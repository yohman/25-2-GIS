# 💻 Lab: Create a Personal Hello World Page in HTML

Welcome to your first web development lab! In this lab, you’ll:

1. Install **Visual Studio Code** (VS Code)  
2. Create a simple **HTML** file  
3. Add your **name, photo, and a short introduction** to the page  

---

## 🧰 Step 1: Install Visual Studio Code

1. Go to the official VS Code website:  
   👉 https://code.visualstudio.com  
2. Click the **Download** button for your operating system (Windows, macOS, Linux).  
3. Open the downloaded file and follow the installation instructions.  
4. Once installed, open **Visual Studio Code**.  

---

## 📁 Step 2: Create Your Project Folder

1. On your computer, create a new folder called:  
   `my-hello-page`  
2. Open **VS Code**.  
3. Go to **File → Open Folder**, and open your new `my-hello-page` folder.  

---

## 📝 Step 3: Create an HTML File

1. In VS Code, click **New File** (or `File → New File`).  
2. Save the file as `index.html`.  
3. Add the following code:


```html
<!DOCTYPE html>
<html>
<head>
  <title>Hello from [Your Name]</title>
</head>
<body>
  <h1>Hello, I'm [Your Name]!</h1>
  <p>Welcome to my first web page. I like coding, music, and playing basketball.</p>

  <img src="me.jpg" width="200" alt="Photo of me">
</body>
</html>
```

```html

<!DOCTYPE html>
<html>
<head>
  <title>Hello from [Your Name]</title>
</head>
<body>
  <h1>Hello, I'm [Your Name]!</h1>
  <p>Welcome to my first web page. I like coding, music, and playing basketball.</p>

  <img src="me.jpg" width="200" alt="Photo of me">
</body>
</html>

```

---

## 🖼️ Step 4: Add Your Photo

1. Find a photo of yourself (can be a selfie or cartoon version).  
2. Rename the image file to `me.jpg` (or `me.png`, but remember to update the HTML too).  
3. Move it into the same folder as your `index.html` file (`my-hello-page`).  

Your folder should look like this:

```
my-hello-page/
├── index.html
└── me.jpg
```

---

## 🌍 Step 5: Open Your Page in a Browser

1. In VS Code, right-click on `index.html` and choose **"Reveal in File Explorer"** (Windows) or **"Reveal in Finder"** (Mac).  
2. Double-click `index.html` to open it in your web browser.  
3. You should see your title, your name, a paragraph, and your photo!

---

## ✅ Challenge

- Try changing the size of your image using `width="300"` or `height="300"`.
- Add more `<p>` tags to tell us more about yourself.

---

## 💡 Tips

- Always save your files after editing (Ctrl+S or Cmd+S).
- Make sure your image is in the **same folder** as your HTML file.
- Use only letters/numbers in filenames (no spaces or symbols).
