
![image](https://github.com/user-attachments/assets/358d6d62-a4b4-4f7e-8431-33e5000b1e16)


# 📘 جزوه کامل آموزش TextField و State متنی در Jetpack Compose + تمرین‌های کاربردی

---

## 1. 🎯 چرا باید با `TextField` و `State` کار کنیم؟

تو اپ‌هایی مثل فرم ثبت‌نام، ورود اطلاعات عددی، وارد کردن اسم، جستجو و حتی ذکرشمار (!) نیاز داریم کاربر **یه چیزی تایپ کنه** و ما با اون ورودی **کاری انجام بدیم**.

برای همین ترکیب `TextField` با `mutableStateOf` و `remember`، از پایه‌های کار با UI توی Composeه.

---

## 2. ✅ مراحل کار با TextField

### مرحله ۱: ساختن state متنی
```kotlin
var text by remember { mutableStateOf("") }
```

### مرحله ۲: نمایش TextField
```kotlin
TextField(
    value = text,
    onValueChange = { text = it }
)
```

---

## 3. 🧰 ویژگی‌های ظاهری TextField

می‌تونی ظاهر فیلد رو تغییر بدی:

| ویژگی | توضیح |
|--------|--------|
| `label` | عنوان بالای فیلد |
| `placeholder` | متن کم‌رنگ راهنما |
| `shape` | گردی گوشه‌ها |
| `colors` | رنگ مرز، متن، لیبل و... |
| `modifier` | اندازه، فاصله و ... |

---

### ✳️ مثال:

```kotlin
OutlinedTextField(
    value = name,
    onValueChange = { name = it },
    label = { Text("نام") },
    placeholder = { Text("مثلاً علی") },
    shape = RoundedCornerShape(12.dp),
    colors = TextFieldDefaults.outlinedTextFieldColors(
        focusedBorderColor = Color.Blue,
        unfocusedBorderColor = Color.Gray
    )
)
```

---

## 4. 🎹 تغییر نوع کیبورد

برای اینکه کاربر مثلاً فقط عدد وارد کنه یا ایمیل، از `keyboardOptions` استفاده می‌کنیم.

### ✳️ مثال:

```kotlin
TextField(
    value = input,
    onValueChange = { input = it },
    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number)
)
```

---

## ✅ تمرین‌های کاربردی + پاسخ

---

### 🧪 تمرین ۱: اپلکیشن محاسبه BMI

**توضیح:**  
کاربر قد (متر) و وزنش (کیلوگرم) رو وارد میکنه و ما BMI یا شاخص سلامتش رو حساب میکنیم. فرمول مربوطه:
وزن تقسیم بر مربع قد


```kotlin
@Composable
fun ZekrShomar() {
    var input by remember { mutableStateOf("") }
    var target by remember { mutableStateOf(0) }
    var count by remember { mutableStateOf(0) }

    Column(modifier = Modifier.padding(16.dp)) {
        OutlinedTextField(
            value = input,
            onValueChange = { input = it },
            label = { Text("تعداد ذکر") },
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number)
        )

        Button(
            onClick = {
                target = input.toIntOrNull() ?: 0
                count = 0
            },
            modifier = Modifier.padding(vertical = 8.dp)
        ) {
            Text("شروع شمارش")
        }

        if (target > 0) {
            Text("تعداد زده‌شده: $count / $target")
            Button(
                onClick = {
                    if (count < target) count++
                },
                modifier = Modifier.padding(top = 8.dp)
            ) {
                Text("ذکر")
            }

            if (count == target && target != 0) {
                Text("✅ تموم شد!", color = Color.Green, fontWeight = FontWeight.Bold)
            }
        }
    }
}
```

---

### 🧪 تمرین ۲: گرفتن ایمیل و نمایش آن با ظاهر سفارشی

```kotlin
@Composable
fun EmailInput() {
    var email by remember { mutableStateOf("") }

    OutlinedTextField(
        value = email,
        onValueChange = { email = it },
        label = { Text("ایمیل") },
        keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email),
        colors = TextFieldDefaults.outlinedTextFieldColors(
            focusedBorderColor = Color(0xFFFF9800), // نارنجی
        ),
        modifier = Modifier
            .fillMaxWidth()
            .padding(16.dp)
    )

    if (email.isNotBlank()) {
        Text("ایمیلی که وارد کردی: $email", modifier = Modifier.padding(16.dp))
    }
}
```

---

### 🧪 تمرین ۳: جمع دو عدد واردشده توسط کاربر

```kotlin
@Composable
fun SumTwoNumbers() {
    var num1 by remember { mutableStateOf("") }
    var num2 by remember { mutableStateOf("") }
    var result by remember { mutableStateOf(0) }

    Column(modifier = Modifier.padding(16.dp)) {
        OutlinedTextField(
            value = num1,
            onValueChange = { num1 = it },
            label = { Text("عدد اول") },
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number)
        )

        OutlinedTextField(
            value = num2,
            onValueChange = { num2 = it },
            label = { Text("عدد دوم") },
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number)
        )

        Button(onClick = {
            val a = num1.toIntOrNull() ?: 0
            val b = num2.toIntOrNull() ?: 0
            result = a + b
        }) {
            Text("جمع کن")
        }

        Text("نتیجه: $result", modifier = Modifier.padding(top = 8.dp))
    }
}
```

---

## 🧠 جمع‌بندی

- با `TextField` می‌تونی از کاربر ورودی بگیری.
- با `mutableStateOf` مقدار ورودی رو نگه‌داری می‌کنی.
- با `onValueChange`، مقدار رو به‌روز می‌کنی.
- با `keyboardOptions` می‌تونی نوع کیبورد رو کنترل کنی.
- تمرین‌های بالا نشون می‌دن چطوری یه اپ کاربردی با همین ابزار ساده بسازی.

---



# 🎓 جزوه آموزشی: لیست پویا با LazyColumn در Jetpack Compose

![image](https://github.com/user-attachments/assets/77b6262a-10a0-477d-920e-6ee2ef3b53d5)


## 📘 مقدمه

در Jetpack Compose برای نمایش لیست‌های پویا و قابل تغییر، از `LazyColumn` همراه با `mutableStateListOf` استفاده می‌کنیم.
این روش به ما اجازه می‌دهد در زمان اجرا آیتم‌ها را اضافه یا حذف کنیم و فوراً تغییرات را در UI ببینیم.

---


## ✅ نمایش لیست پویا با `mutableStateListOf`

```kotlin
@Composable
fun DynamicListExample() {
    val items = remember { mutableStateListOf<String>() }
    var text by remember { mutableStateOf("") }

    Column(modifier = Modifier.padding(16.dp)) {
        TextField(
            value = text,
            onValueChange = { text = it },
            label = { Text("یک متن وارد کن") },
            modifier = Modifier.fillMaxWidth()
        )

        Spacer(modifier = Modifier.height(8.dp))

        Button(
            onClick = {
                items.add(text)
                text = ""
            }
        ) {
            Text("افزودن به لیست")
        }

        Spacer(modifier = Modifier.height(16.dp))

        LazyColumn {
            items(items) { item ->
                Text(
                    text = item,
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(8.dp),
                    fontSize = 18.sp
                )
            }
        }
    }
}
```

---

## 🎯 تمرین: بازی حدس عدد

### ✍️ صورت تمرین:

یک عدد مخفی در برنامه تولید شود.
کاربر عددی را حدس می‌زند و پس از هر حدس، برنامه می‌گوید عدد بزرگ‌تر است، کوچک‌تر است یا درست حدس زده شده.
همه‌ی حدس‌ها در یک لیست داینامیک نمایش داده شوند.

---

## ✅ راه‌حل تمرین با `mutableStateListOf`

```kotlin
@Composable
fun SimpleNumberGuessGame() {
    val guesses = remember { mutableStateListOf<String>() }
    var input by remember { mutableStateOf("") }
    var message by remember { mutableStateOf("") }
    val secretNumber = remember { Random.nextInt(1, 101) }

    Column(modifier = Modifier.padding(16.dp)) {
        TextField(
            value = input,
            onValueChange = { input = it },
            label = { Text("یک عدد حدس بزن") },
            modifier = Modifier.fillMaxWidth()
        )

        Spacer(modifier = Modifier.height(8.dp))

        Button(onClick = {
            val guess = input.toInt()
            guesses.add("حدس: $guess")
            message = when {
                guess < secretNumber -> "عدد بزرگ‌تره"
                guess > secretNumber -> "عدد کوچک‌تره"
                else -> "درست گفتی!"
            }
            input = ""
        }) {
            Text("ثبت حدس")
        }

        Spacer(modifier = Modifier.height(8.dp))
        Text(text = message, fontSize = 18.sp)

        LazyColumn {
            items(guesses) { guessText ->
                Text(
                    text = guessText,
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(4.dp)
                )
            }
        }
    }
}
```

---

## 🧠 نکات مهم

* `mutableStateListOf`: لیستی که با تغییر محتوای آن، UI به‌صورت خودکار به‌روزرسانی می‌شود.
* `remember`: برای نگهداری وضعیت در طول عمر کامپوز.
* `LazyColumn + items(list)`: برای نمایش عناصر پویا و قابل رشد.

---

## ✅ جمع‌بندی

`mutableStateListOf` ابزار اصلی برای ساخت لیست‌های داینامیک در Jetpack Compose است.
با استفاده از آن، می‌توان اپلیکیشن‌های تعاملی مثل لیست، فرم، یا بازی‌های ساده طراحی کرد.

---

