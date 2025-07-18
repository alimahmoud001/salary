<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>حساب الراتب والمصاريف الشهرية</title>
    <script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>
    <style>
        :root {
            --primary-color: #3498db;
            --secondary-color: #2980b9;
            --success-color: #2ecc71;
            --danger-color: #e74c3c;
            --warning-color: #f39c12;
            --light-color: #ecf0f1;
            --dark-color: #2c3e50;
            --border-radius: 8px;
            --box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            --transition: all 0.3s ease;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            color: var(--dark-color);
            background-color: #f5f7fa;
            padding: 0;
            margin: 0;
        }

        .container {
            width: 100%;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        h1, h2, h3 {
            color: var(--dark-color);
            margin-bottom: 15px;
            text-align: center;
        }

        h1 {
            font-size: 1.8rem;
            margin: 20px 0;
            color: var(--primary-color);
        }

        h2 {
            font-size: 1.4rem;
            border-bottom: 2px solid var(--primary-color);
            padding-bottom: 10px;
            margin-top: 30px;
        }

        .section {
            background-color: white;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            padding: 20px;
            margin-bottom: 25px;
            transition: var(--transition);
        }

        .section:hover {
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
        }

        .form-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
            color: var(--dark-color);
        }

        input, select, button {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: var(--border-radius);
            font-size: 16px;
            transition: var(--transition);
        }

        input:focus, select:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 0 2px rgba(52, 152, 219, 0.2);
        }

        button {
            background-color: var(--primary-color);
            color: white;
            border: none;
            cursor: pointer;
            font-weight: 600;
            margin-top: 10px;
            padding: 12px 20px;
        }

        button:hover {
            background-color: var(--secondary-color);
            transform: translateY(-2px);
        }

        button:active {
            transform: translateY(0);
        }

        .btn-danger {
            background-color: var(--danger-color);
        }

        .btn-danger:hover {
            background-color: #c0392b;
        }

        .btn-success {
            background-color: var(--success-color);
        }

        .btn-success:hover {
            background-color: #27ae60;
        }

        .btn-warning {
            background-color: var(--warning-color);
        }

        .btn-warning:hover {
            background-color: #d35400;
        }

        .total {
            font-weight: bold;
            font-size: 1.2rem;
            color: var(--danger-color);
            text-align: center;
            margin: 15px 0;
            padding: 10px;
            background-color: var(--light-color);
            border-radius: var(--border-radius);
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin: 15px 0;
            font-size: 0.9rem;
            box-shadow: var(--box-shadow);
            border-radius: var(--border-radius);
            overflow: hidden;
        }

        th, td {
            padding: 12px 15px;
            text-align: center;
            border: 1px solid #ddd;
        }

        th {
            background-color: var(--primary-color);
            color: white;
            font-weight: 600;
        }

        tr:nth-child(even) {
            background-color: #f8f9fa;
        }

        tr:hover {
            background-color: #f1f1f1;
        }

        .category-header {
            background-color: var(--primary-color);
            color: white;
            padding: 12px 15px;
            margin: 20px 0 5px 0;
            border-radius: var(--border-radius);
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-weight: 600;
        }

        .category-header:hover {
            background-color: var(--secondary-color);
        }

        .sub-items {
            margin-left: 0;
            overflow: hidden;
            transition: max-height 0.3s ease;
            max-height: 0;
        }

        .sub-items.show {
            max-height: 5000px;
        }

        .invoice-container {
            background-color: white;
            padding: 20px;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            margin-top: 20px;
        }

        .invoice-header {
            text-align: center;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid var(--primary-color);
        }

        .invoice-footer {
            text-align: center;
            margin-top: 20px;
            padding-top: 10px;
            border-top: 2px solid var(--primary-color);
            font-weight: bold;
        }

        .actions {
            display: flex;
            gap: 10px;
            justify-content: center;
            margin-top: 20px;
        }

        .actions button {
            flex: 1;
            max-width: 200px;
        }

        @media (min-width: 768px) {
            .form-row {
                display: flex;
                gap: 15px;
            }

            .form-row .form-group {
                flex: 1;
            }

            h1 {
                font-size: 2.2rem;
            }

            h2 {
                font-size: 1.6rem;
            }

            table {
                font-size: 1rem;
            }
        }

        @media print {
            body * {
                visibility: hidden;
            }
            .invoice-container, .invoice-container * {
                visibility: visible;
            }
            .invoice-container {
                position: absolute;
                left: 0;
                top: 0;
                width: 100%;
                box-shadow: none;
            }
            .no-print {
                display: none;
            }
        }

        /* Animation for toggling categories */
        @keyframes slideDown {
            from {
                opacity: 0;
                transform: translateY(-10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .animate {
            animation: slideDown 0.3s ease forwards;
        }

        /* Badge for totals */
        .badge {
            display: inline-block;
            padding: 3px 7px;
            font-size: 12px;
            font-weight: bold;
            line-height: 1;
            color: white;
            text-align: center;
            white-space: nowrap;
            vertical-align: middle;
            background-color: var(--primary-color);
            border-radius: 10px;
            margin-left: 5px;
        }

        /* Responsive table */
        .table-responsive {
            overflow-x: auto;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>نظام حساب الراتب والمصاريف الشهرية</h1>
        
        <div class="section salary-section">
            <h2>الراتب الشهري</h2>
            <div class="form-row">
                <div class="form-group">
                    <label for="salary">مبلغ الراتب (ل.س):</label>
                    <input type="number" id="salary" placeholder="أدخل مبلغ الراتب">
                </div>
                <div class="form-group">
                    <label>&nbsp;</label>
                    <button onclick="updateSalary()" class="btn-success">حفظ الراتب</button>
                </div>
            </div>
            <div id="salary-display" class="total"></div>
        </div>
        
        <div class="section">
            <h2>إضافة مصروفات جديدة</h2>
            <div class="form-row">
                <div class="form-group">
                    <label for="category">الفئة الرئيسية:</label>
                    <select id="category">
                        <option value="منظفات">منظفات</option>
                        <option value="ملابس">ملابس</option>
                        <option value="مشروبات">مشروبات</option>
                        <option value="اكل">اكل</option>
                        <option value="محروقات">محروقات</option>
                        <option value="مدونة">مدونة</option>
                        <option value="اجار طريق">اجار طريق</option>
                        <option value="فواتير">فواتير</option>
                        <option value="أخرى">أخرى</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="subcategory">البند الفرعي:</label>
                    <input type="text" id="subcategory" placeholder="أدخل البند الفرعي">
                </div>
            </div>
            
            <div class="form-row">
                <div class="form-group">
                    <label for="price">السعر (ل.س):</label>
                    <input type="number" id="price" placeholder="السعر">
                </div>
                <div class="form-group">
                    <label for="quantity">الكمية:</label>
                    <input type="number" id="quantity" value="1" placeholder="الكمية">
                </div>
                <div class="form-group">
                    <label for="date">التاريخ:</label>
                    <input type="date" id="date">
                </div>
            </div>
            
            <button onclick="addExpense()" class="btn-success">إضافة مصروف</button>
        </div>
        
        <div class="section">
            <h2>ملخص المصروفات حسب الفئات</h2>
            <div id="expenses-summary"></div>
            <div id="remaining-salary" class="total"></div>
        </div>
        
        <div class="section">
            <h2>كافة المصروفات</h2>
            <div id="all-expenses"></div>
        </div>
        
        <div class="section">
            <h2>إنشاء فاتورة يومية</h2>
            <div class="form-row">
                <div class="form-group">
                    <label for="invoice-date">اختر تاريخ الفاتورة:</label>
                    <input type="date" id="invoice-date">
                </div>
                <div class="form-group">
                    <label>&nbsp;</label>
                    <button onclick="generateDailyInvoice()" class="btn-success">إنشاء الفاتورة</button>
                </div>
            </div>
            
            <div id="invoice-result" class="invoice-container" style="display: none;">
                <div id="invoice-content"></div>
                <div class="actions no-print">
                    <button onclick="printInvoice()" class="btn-warning">طباعة الفاتورة</button>
                    <button onclick="saveAsImage()" class="btn-success">حفظ كصورة</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        // تخزين البيانات
        let monthlySalary = 0;
        let expenses = [];
        
        // تحميل البيانات من localStorage عند بدء التشغيل
        window.onload = function() {
            loadData();
            updateSalaryDisplay();
            updateExpensesDisplay();
            
            // تعيين التاريخ الحالي كقيمة افتراضية
            const today = new Date().toISOString().split('T')[0];
            document.getElementById('date').value = today;
            document.getElementById('invoice-date').value = today;
        };
        
        // تحديث الراتب
        function updateSalary() {
            const salaryInput = document.getElementById('salary');
            monthlySalary = parseFloat(salaryInput.value) || 0;
            
            if (monthlySalary <= 0) {
                alert('الرجاء إدخال مبلغ راتب صحيح');
                return;
            }
            
            localStorage.setItem('monthlySalary', monthlySalary);
            updateSalaryDisplay();
            updateExpensesDisplay();
            salaryInput.value = '';
            
            // إظهار رسالة نجاح
            showToast('تم حفظ الراتب بنجاح', 'success');
        }
        
        // عرض الراتب
        function updateSalaryDisplay() {
            const salaryDisplay = document.getElementById('salary-display');
            salaryDisplay.textContent = `الراتب الشهري: ${formatNumber(monthlySalary)} ليرة سورية`;
        }
        
        // إضافة مصروف جديد
        function addExpense() {
            const category = document.getElementById('category').value;
            const subcategory = document.getElementById('subcategory').value.trim();
            const price = parseFloat(document.getElementById('price').value) || 0;
            const quantity = parseInt(document.getElementById('quantity').value) || 1;
            const date = document.getElementById('date').value;
            
            if (!category || !subcategory || price <= 0) {
                showToast('الرجاء إدخال بيانات صحيحة', 'error');
                return;
            }
            
            if (!date) {
                showToast('الرجاء تحديد تاريخ المصروف', 'error');
                return;
            }
            
            const total = price * quantity;
            const expense = {
                id: Date.now(),
                category,
                subcategory,
                price,
                quantity,
                total,
                date: formatDateForDisplay(date)
            };
            
            expenses.push(expense);
            saveData();
            updateExpensesDisplay();
            
            // مسح حقول الإدخال
            document.getElementById('subcategory').value = '';
            document.getElementById('price').value = '';
            document.getElementById('quantity').value = '1';
            
            // إظهار رسالة نجاح
            showToast('تم إضافة المصروف بنجاح', 'success');
        }
        
        // حذف مصروف
        function removeExpense(id) {
            if (confirm('هل أنت متأكد من حذف هذا المصروف؟')) {
                expenses = expenses.filter(expense => expense.id !== id);
                saveData();
                updateExpensesDisplay();
                showToast('تم حذف المصروف بنجاح', 'success');
            }
        }
        
        // حفظ البيانات في localStorage
        function saveData() {
            localStorage.setItem('expenses', JSON.stringify(expenses));
        }
        
        // تحميل البيانات من localStorage
        function loadData() {
            const savedSalary = localStorage.getItem('monthlySalary');
            if (savedSalary) {
                monthlySalary = parseFloat(savedSalary);
            }
            
            const savedExpenses = localStorage.getItem('expenses');
            if (savedExpenses) {
                expenses = JSON.parse(savedExpenses);
            }
        }
        
        // تحديث عرض المصروفات
        function updateExpensesDisplay() {
            updateExpensesSummary();
            updateAllExpensesList();
            updateRemainingSalary();
        }
        
        // عرض ملخص المصروفات حسب الفئة
        function updateExpensesSummary() {
            const summaryElement = document.getElementById('expenses-summary');
            
            // تجميع المصروفات حسب الفئة
            const categories = {};
            expenses.forEach(expense => {
                if (!categories[expense.category]) {
                    categories[expense.category] = 0;
                }
                categories[expense.category] += expense.total;
            });
            
            // إنشاء الجدول
            let html = '<div class="table-responsive"><table>';
            html += '<tr><th>الفئة</th><th>المجموع (ل.س)</th></tr>';
            
            for (const category in categories) {
                html += `<tr><td>${category}</td><td>${formatNumber(categories[category])}</td></tr>`;
            }
            
            // حساب المجموع الكلي
            const totalExpenses = expenses.reduce((sum, expense) => sum + expense.total, 0);
            html += `<tr class="total"><td>المجموع الكلي</td><td>${formatNumber(totalExpenses)}</td></tr>`;
            
            html += '</table></div>';
            summaryElement.innerHTML = html;
        }
        
        // عرض كافة المصروفات
        function updateAllExpensesList() {
            const allExpensesElement = document.getElementById('all-expenses');
            
            if (expenses.length === 0) {
                allExpensesElement.innerHTML = '<p style="text-align: center; color: #777;">لا توجد مصروفات مسجلة</p>';
                return;
            }
            
            // تجميع المصروفات حسب الفئة
            const expensesByCategory = {};
            expenses.forEach(expense => {
                if (!expensesByCategory[expense.category]) {
                    expensesByCategory[expense.category] = [];
                }
                expensesByCategory[expense.category].push(expense);
            });
            
            let html = '';
            
            for (const category in expensesByCategory) {
                const categoryTotal = expensesByCategory[category].reduce((sum, expense) => sum + expense.total, 0);
                
                html += `
                    <div class="category-header" onclick="toggleCategory('${category}')">
                        <span>${category} <span class="badge">${expensesByCategory[category].length}</span></span>
                        <span>${formatNumber(categoryTotal)} ل.س</span>
                    </div>
                `;
                
                html += `<div id="${category}-items" class="sub-items">`;
                html += '<div class="table-responsive"><table>';
                html += '<tr><th>التاريخ</th><th>البند الفرعي</th><th>السعر</th><th>الكمية</th><th>المجموع</th><th>إجراءات</th></tr>';
                
                expensesByCategory[category].forEach(expense => {
                    html += `
                        <tr>
                            <td>${expense.date}</td>
                            <td>${expense.subcategory}</td>
                            <td>${formatNumber(expense.price)}</td>
                            <td>${expense.quantity}</td>
                            <td>${formatNumber(expense.total)}</td>
                            <td><button class="remove-btn btn-danger" onclick="removeExpense(${expense.id})">حذف</button></td>
                        </tr>
                    `;
                });
                
                html += '</table></div></div>';
            }
            
            allExpensesElement.innerHTML = html;
        }
        
        // عرض الراتب المتبقي
        function updateRemainingSalary() {
            const totalExpenses = expenses.reduce((sum, expense) => sum + expense.total, 0);
            const remaining = monthlySalary - totalExpenses;
            const remainingElement = document.getElementById('remaining-salary');
            
            remainingElement.textContent = `الراتب المتبقي: ${formatNumber(remaining)} ليرة سورية`;
            
            if (remaining < 0) {
                remainingElement.style.color = 'var(--danger-color)';
                remainingElement.innerHTML += ' <span style="color: var(--dark-color);">(تجاوز الميزانية)</span>';
            } else {
                remainingElement.style.color = 'var(--success-color)';
            }
        }
        
        // إنشاء فاتورة يومية
        function generateDailyInvoice() {
            const invoiceDateInput = document.getElementById('invoice-date').value;
            const invoiceResult = document.getElementById('invoice-result');
            const invoiceContent = document.getElementById('invoice-content');
            
            if (!invoiceDateInput) {
                showToast('الرجاء اختيار تاريخ الفاتورة', 'error');
                return;
            }
            
            const invoiceDate = formatDateForDisplay(invoiceDateInput);
            const dailyExpenses = expenses.filter(expense => expense.date === invoiceDate);
            
            if (dailyExpenses.length === 0) {
                invoiceResult.style.display = 'none';
                showToast('لا توجد مصروفات مسجلة لهذا اليوم', 'warning');
                return;
            }
            
            let html = `
                <div class="invoice-header">
                    <h3>فاتورة مصروفات يومية</h3>
                    <p>تاريخ الفاتورة: ${invoiceDate}</p>
                </div>
                <div class="table-responsive">
                    <table>
                        <tr>
                            <th>#</th>
                            <th>الفئة</th>
                            <th>البند الفرعي</th>
                            <th>السعر</th>
                            <th>الكمية</th>
                            <th>المجموع</th>
                        </tr>
            `;
            
            let total = 0;
            dailyExpenses.forEach((expense, index) => {
                html += `
                    <tr>
                        <td>${index + 1}</td>
                        <td>${expense.category}</td>
                        <td>${expense.subcategory}</td>
                        <td>${formatNumber(expense.price)}</td>
                        <td>${expense.quantity}</td>
                        <td>${formatNumber(expense.total)}</td>
                    </tr>
                `;
                total += expense.total;
            });
            
            html += `
                    </table>
                </div>
                <div class="invoice-footer">
                    <p>المجموع الكلي: ${formatNumber(total)} ليرة سورية</p>
                </div>
            `;
            
            invoiceContent.innerHTML = html;
            invoiceResult.style.display = 'block';
            
            // التمرير إلى قسم الفاتورة
            invoiceResult.scrollIntoView({ behavior: 'smooth' });
        }
        
        // طباعة الفاتورة
        function printInvoice() {
            window.print();
        }
        
        // حفظ الفاتورة كصورة
        function saveAsImage() {
            const invoiceElement = document.getElementById('invoice-content');
            
            html2canvas(invoiceElement, {
                scale: 2,
                logging: false,
                useCORS: true,
                allowTaint: true
            }).then(canvas => {
                const link = document.createElement('a');
                link.download = 'فاتورة-مصروفات-' + document.getElementById('invoice-date').value + '.png';
                link.href = canvas.toDataURL('image/png');
                link.click();
                showToast('تم حفظ الفاتورة كصورة', 'success');
            });
        }
        
        // تبديل عرض/إخفاء الفئة
        function toggleCategory(category) {
            const element = document.getElementById(`${category}-items`);
            const isVisible = element.classList.contains('show');
            
            if (isVisible) {
                element.classList.remove('show');
                element.style.maxHeight = '0';
            } else {
                element.classList.add('show', 'animate');
                element.style.maxHeight = `${element.scrollHeight}px`;
            }
        }
        
        // تنسيق الأرقام بفواصل
        function formatNumber(num) {
            return num.toLocaleString('ar-SY');
        }
        
        // تنسيق التاريخ للعرض
        function formatDateForDisplay(dateString) {
            const date = new Date(dateString);
            return date.toLocaleDateString('ar-SY', {
                year: 'numeric',
                month: 'long',
                day: 'numeric'
            });
        }
        
        // إظهار رسالة toast
        function showToast(message, type) {
            const toast = document.createElement('div');
            toast.className = `toast toast-${type}`;
            toast.textContent = message;
            document.body.appendChild(toast);
            
            setTimeout(() => {
                toast.classList.add('show');
            }, 10);
            
            setTimeout(() => {
                toast.classList.remove('show');
                setTimeout(() => {
                    document.body.removeChild(toast);
                }, 300);
            }, 3000);
        }
        
        // إضافة أنماط الـ toast ديناميكيًا
        const toastStyles = document.createElement('style');
        toastStyles.textContent = `
            .toast {
                position: fixed;
                bottom: 20px;
                left: 50%;
                transform: translateX(-50%);
                padding: 12px 24px;
                border-radius: var(--border-radius);
                color: white;
                font-weight: bold;
                z-index: 1000;
                opacity: 0;
                transition: opacity 0.3s ease;
                box-shadow: var(--box-shadow);
            }
            .toast.show {
                opacity: 1;
            }
            .toast-success {
                background-color: var(--success-color);
            }
            .toast-error {
                background-color: var(--danger-color);
            }
            .toast-warning {
                background-color: var(--warning-color);
            }
        `;
        document.head.appendChild(toastStyles);
    </script>
</body>
</html>
