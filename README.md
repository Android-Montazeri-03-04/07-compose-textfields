
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

### 🧪 تمرین ۱: اپلیکیشن ذکر شمار با تعداد دلخواه کاربر

**توضیح:**  
کاربر توی TextField یه عدد وارد می‌کنه (مثلاً 33)، بعد یه دکمه می‌زنه و با هر کلیک روی دکمه‌ی "ذکر"، یه شمارنده بالا می‌ره. وقتی به تعداد هدف رسید، متن «تموم شد» نشون داده می‌شه.

**پاسخ:**

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

