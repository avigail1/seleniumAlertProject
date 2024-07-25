# Alert Project Automation

## Objective

This project demonstrates how to use Selenium with C# to automate tests for handling alerts and switching between windows using NUnit.

## Introduction

Selenium is an open-source framework used for automating web browsers. It supports various programming languages, including C#. With Selenium, you can write scripts to interact with web elements, simulate user actions, and verify the behavior of web applications. Selenium is widely used for functional and regression testing.

In this project, we use Selenium WebDriver to control the Chrome browser and NUnit as our test framework. Selenium WebDriver provides a programming interface to create and execute test scripts, while NUnit helps in organizing and running tests.

## Tools and Technologies

- Selenium WebDriver
- ChromeDriver
- NUnit
- Visual Studio
- C#

## Exercise Steps

### Part 1: Setup

#### Install Visual Studio

Ensure you have Visual Studio installed on your machine.

#### Create a New Project

1. Open Visual Studio.
2. Create a new NUnit project.
3. Name the project `AlertProject`.

#### Add NuGet Packages

1. Right-click on the project in Solution Explorer and select "Manage NuGet Packages".
2. Install the following packages:
   - `Selenium.WebDriver`
   - `Selenium.WebDriver.ChromeDriver`
   - `Selenium.Support`

### Part 2: ChromeDriver Installation

#### Download ChromeDriver

1. Go to the [ChromeDriver Downloads](https://sites.google.com/a/chromium.org/chromedriver/downloads) page.
2. Download the ChromeDriver version that matches your Chrome browser version.

#### Install ChromeDriver

1. Extract the downloaded `chromedriver.zip` file.
2. Place the `chromedriver.exe` file in a directory of your choice (e.g., `E:\אוטומציה\AlertProject\AlertProject\drivers`).

#### Set ChromeDriver Path

Optionally, add the directory containing `chromedriver.exe` to your system PATH environment variable. Alternatively, you can directly reference the path in your test code.

### Part 3: Writing the Tests

#### Create Test Class

1. Create a new folder named `Tests` in your project.
2. Add a new class named `Tests` in the `Tests` folder.

#### Write the Test Methods

Implement the test methods to perform the following steps:

1. **Wait for Alert**:
   - Navigate to the demo page for alerts.
   - Click the button to trigger an alert after a delay.
   - Wait for the alert to appear.
   - Accept the alert.

2. **Switch Between Windows**:
   - Navigate to the demo page for browser windows.
   - Click the button to open a new window.
   - Switch to the new window and verify the URL.
   - Close the new window and switch back to the original window.

### Example Code

```csharp
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using OpenQA.Selenium.Support.UI;
using SeleniumExtras.WaitHelpers;
using NUnit.Framework;
using System;
using System.Collections.Generic;

namespace AlertProject
{
    [TestFixture]
    public class Tests
    {
        private IWebDriver driver;

        [SetUp]
        public void Setup()
        {
            string path = "E:\\אוטומציה\\AlertProject\\AlertProject";
            driver = new ChromeDriver(path + @"\drivers");
        }

        [Test]
        public void WaitToAlert()
        {
            driver.Navigate().GoToUrl("https://demoqa.com/alerts");

            IWebElement button = driver.FindElement(By.Id("timerAlertButton"));
            button.Click();

            WebDriverWait wait = new WebDriverWait(driver, TimeSpan.FromSeconds(10));
            wait.Until(ExpectedConditions.AlertIsPresent());

            IAlert alert = driver.SwitchTo().Alert();
            alert.Accept();
        }

        [Test]
        public void PassBetweenWindows()
        {
            driver.Navigate().GoToUrl("https://demoqa.com/browser-windows");

            IWebElement button = driver.FindElement(By.Id("windowButton"));
            string currentWindow = driver.CurrentWindowHandle.ToString();
            button.Click();

            List<string> windowHandles = new List<string>(driver.WindowHandles);
            driver.SwitchTo().Window(windowHandles[1]);

            Assert.AreEqual(driver.Url, "https://demoqa.com/sample");

            ((IJavaScriptExecutor)driver).ExecuteScript("window.close();");
            driver.SwitchTo().Window(currentWindow);
        }

        [TearDown]
        public void TearDown()
        {
            driver.Dispose();
        }
    }
}
