<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>حساب الراتب والمصاريف الشهرية</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f5f5f5;
        }
        .container {
            max-width: 1000px;
            margin: 0 auto;
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        h1, h2 {
            color: #2c3e50;
            text-align: center;
        }
        .section {
            margin-bottom: 30px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin: 10px 0;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: center;
        }
        th {
            background-color: #f2f2f2;
        }
        input, select, button {
            padding: 8px;
            margin: 5px 0;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            background-color: #4CAF50;
            color: white;
            cursor: pointer;
            border: none;
        }
        button:hover {
            background-color: #45a049;
        }
        .total {
            font-weight: bold;
            font-size: 1.2em;
            color: #e74c3c;
        }
        .invoice {
            background-color: #fff9e6;
            padding: 15px;
            margin-top: 20px;
            border: 1px dashed #ccc;
        }
        .remove-btn {
            background-color: #e74c3c;
        }
        .remove-btn:hover {
            background-color: #c0392b;
        }
        .category-header {
            background-color: #3498db;
            color: white;
            padding: 10px;
            margin-top: 20px;
            border-radius: 5px;
            cursor: pointer;
        }
        .sub-items {
            margin-left: 20px;
            display: none;
        }
        .salary-section {
            background-color: #e8f4f8;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>نظام حساب الراتب والمصاريف الشهرية</h1>
        
        <div class="section salary-section">
            <h2>الراتب الشهري</h2>
            <div>
                <label for="salary">مبلغ الراتب (ل.س):</label>
                <input type="number" id="salary" placeholder="أدخل مبلغ الراتب">
                <button onclick="updateSalary()">حفظ الراتب</button>
            </div>
            <div id="salary-display" class="total"></div>
        </div>
        
        <div class="section">
            <h2>إضافة مصروفات جديدة</h2>
            <div>
                <label for="category">الفئة الرئيسية:</label>
                <select id="category">
                    <option value="منظفات">منظفات</option>
                    <option value="ملابس">ملابس</option>
                    <option value="مشروبات">مشروبات</option>
                    <option value="اكل">اكل</option>
                    <option value="محروقات">محروقات</option>
                    <option value="مدونة">مونة</option>
                    <option value="اجار طريق">اجار طريق</option>
                    <option value="فواتير">فواتير</option>
                    <option value="أخرى">أخرى</option>
                </select>
                
                <label for="subcategory">البند الفرعي:</label>
                <input type="text" id="subcategory" placeholder="أدخل البند الفرعي">
                
                <label for="price">السعر (ل.س):</label>
                <input type="number" id="price" placeholder="السعر">
                
                <label for="quantity">الكمية:</label>
                <input type="number" id="quantity" value="1" placeholder="الكمية">
                
                <label for="date">التاريخ:</label>
                <input type="date" id="date">
                
                <button onclick="addExpense()">إضافة مصروف</button>
            </div>
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
        
        <div class="section invoice">
            <h2>إنشاء فاتورة يومية</h2>
            <div>
                <label for="invoice-date">اختر تاريخ الفاتورة:</label>
                <input type="date" id="invoice-date">
                <button onclick="generateDailyInvoice()">إنشاء الفاتورة</button>
            </div>
            <div id="invoice-result"></div>
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
            document.getElementById('date').valueAsDate = new Date();
            document.getElementById('invoice-date').valueAsDate = new Date();
        };
        
        // تحديث الراتب
        function updateSalary() {
            const salaryInput = document.getElementById('salary');
            monthlySalary = parseFloat(salaryInput.value) || 0;
            localStorage.setItem('monthlySalary', monthlySalary);
            updateSalaryDisplay();
            updateExpensesDisplay();
            salaryInput.value = '';
        }
        
        // عرض الراتب
        function updateSalaryDisplay() {
            const salaryDisplay = document.getElementById('salary-display');
            salaryDisplay.textContent = `الراتب الشهري: ${formatNumber(monthlySalary)} ل.س`;
        }
        
        // إضافة مصروف جديد
        function addExpense() {
            const category = document.getElementById('category').value;
            const subcategory = document.getElementById('subcategory').value;
            const price = parseFloat(document.getElementById('price').value) || 0;
            const quantity = parseInt(document.getElementById('quantity').value) || 1;
            const date = document.getElementById('date').value;
            
            if (!category || !subcategory || price <= 0) {
                alert('الرجاء إدخال بيانات صحيحة');
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
                date
            };
            
            expenses.push(expense);
            saveData();
            updateExpensesDisplay();
            
            // مسح حقول الإدخال
            document.getElementById('subcategory').value = '';
            document.getElementById('price').value = '';
            document.getElementById('quantity').value = '1';
        }
        
        // حذف مصروف
        function removeExpense(id) {
            expenses = expenses.filter(expense => expense.id !== id);
            saveData();
            updateExpensesDisplay();
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
            let html = '<table>';
            html += '<tr><th>الفئة</th><th>المجموع (ل.س)</th></tr>';
            
            for (const category in categories) {
                html += `<tr><td>${category}</td><td>${formatNumber(categories[category])}</td></tr>`;
            }
            
            // حساب المجموع الكلي
            const totalExpenses = expenses.reduce((sum, expense) => sum + expense.total, 0);
            html += `<tr class="total"><td>المجموع الكلي</td><td>${formatNumber(totalExpenses)}</td></tr>`;
            
            html += '</table>';
            summaryElement.innerHTML = html;
        }
        
        // عرض كافة المصروفات
        function updateAllExpensesList() {
            const allExpensesElement = document.getElementById('all-expenses');
            
            if (expenses.length === 0) {
                allExpensesElement.innerHTML = '<p>لا توجد مصروفات مسجلة</p>';
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
                html += `<div class="category-header" onclick="toggleCategory('${category}')">${category} ▼</div>`;
                html += `<div id="${category}-items" class="sub-items">`;
                html += '<table>';
                html += '<tr><th>التاريخ</th><th>البند الفرعي</th><th>السعر</th><th>الكمية</th><th>المجموع</th><th>إجراءات</th></tr>';
                
                expensesByCategory[category].forEach(expense => {
                    html += `
                        <tr>
                            <td>${expense.date}</td>
                            <td>${expense.subcategory}</td>
                            <td>${formatNumber(expense.price)}</td>
                            <td>${expense.quantity}</td>
                            <td>${formatNumber(expense.total)}</td>
                            <td><button class="remove-btn" onclick="removeExpense(${expense.id})">حذف</button></td>
                        </tr>
                    `;
                });
                
                // حساب مجموع الفئة
                const categoryTotal = expensesByCategory[category].reduce((sum, expense) => sum + expense.total, 0);
                html += `<tr class="total"><td colspan="4">المجموع</td><td>${formatNumber(categoryTotal)}</td><td></td></tr>`;
                
                html += '</table></div>';
            }
            
            allExpensesElement.innerHTML = html;
        }
        
        // عرض الراتب المتبقي
        function updateRemainingSalary() {
            const totalExpenses = expenses.reduce((sum, expense) => sum + expense.total, 0);
            const remaining = monthlySalary - totalExpenses;
            const remainingElement = document.getElementById('remaining-salary');
            
            remainingElement.textContent = `الراتب المتبقي: ${formatNumber(remaining)} ل.س`;
            
            if (remaining < 0) {
                remainingElement.style.color = 'red';
            } else {
                remainingElement.style.color = 'green';
            }
        }
        
        // إنشاء فاتورة يومية
        function generateDailyInvoice() {
            const invoiceDate = document.getElementById('invoice-date').value;
            const invoiceResult = document.getElementById('invoice-result');
            
            if (!invoiceDate) {
                alert('الرجاء اختيار تاريخ الفاتورة');
                return;
            }
            
            const dailyExpenses = expenses.filter(expense => expense.date === invoiceDate);
            
            if (dailyExpenses.length === 0) {
                invoiceResult.innerHTML = '<p>لا توجد مصروفات مسجلة لهذا اليوم</p>';
                return;
            }
            
            let html = `<h3>فاتورة يوم ${invoiceDate}</h3>`;
            html += '<table>';
            html += '<tr><th>الفئة</th><th>البند الفرعي</th><th>السعر</th><th>الكمية</th><th>المجموع</th></tr>';
            
            let total = 0;
            dailyExpenses.forEach(expense => {
                html += `
                    <tr>
                        <td>${expense.category}</td>
                        <td>${expense.subcategory}</td>
                        <td>${formatNumber(expense.price)}</td>
                        <td>${expense.quantity}</td>
                        <td>${formatNumber(expense.total)}</td>
                    </tr>
                `;
                total += expense.total;
            });
            
            html += `<tr class="total"><td colspan="4">المجموع الكلي</td><td>${formatNumber(total)} ل.س</td></tr>`;
            html += '</table>';
            
            // إضافة زر لطباعة الفاتورة
            html += '<button onclick="window.print()">طباعة الفاتورة</button>';
            
            invoiceResult.innerHTML = html;
        }
        
        // تبديل عرض/إخفاء الفئة
        function toggleCategory(category) {
            const element = document.getElementById(`${category}-items`);
            if (element.style.display === 'block') {
                element.style.display = 'none';
                document.querySelector(`.category-header:contains("${category}")`).textContent = `${category} ▼`;
            } else {
                element.style.display = 'block';
                document.querySelector(`.category-header:contains("${category}")`).textContent = `${category} ▲`;
            }
        }
        
        // تنسيق الأرقام بفواصل
        function formatNumber(num) {
            return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
        }
    </script>
</body>
</html>
