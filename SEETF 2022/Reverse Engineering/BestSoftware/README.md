# 🧑‍🎓 BestSoftware

**Category**: Reversing

100 points | 160 solves

----

## Challenge Description

Help! I purchased the license for a software called BestSoftware. However, I forgot the license key to it, could you help me?

My name is "seetf" and my email is "seetf@seetf.sg". Thank you!

For beginners:

* [https://www.jetbrains.com/decompiler/](https://www.jetbrains.com/decompiler/)

## Attached files/Instructions

* BestSoftware.exe

----

## Solution

We can use the linked JetBrains decompiler, dotPeek, to decompile C# programs, such as `BestSoftware.exe`.

![Screenshot of BestSoftware.exe decompiled using dotPeek](https://user-images.githubusercontent.com/40383042/173202037-bb2b519d-19ba-4316-ab80-614c7b1cfa0b.png)

Looking at the source code, we can see the license key checker code:

```csharp
    public static bool CheckLicenseKey(string name, string email, string licenseKey)
    {
      string shA256 = Program.CalculateSHA256(name + "1_l0v3_CSh4rp" + email);
      return licenseKey.Equals(shA256);
    }
```

A valid license key for the name `seetf` and email `seetf@seetf.sg` is the SHA256 of `seetf1_l0v3_CSh4rpseetf@seetf.sg`.

To calculate the SHA256, we can use online tools such as [CyberChef](https://cyberchef.org/) or [convertstring.com](https://www.convertstring.com/Hash/SHA256). This gives us the hash (license key) `28F313A48C1282DF95E07BCEF466D19517587BCAB4F7A78532FA54AC6708444E`.

### Flag

```text
SEE{28F313A48C1282DF95E07BCEF466D19517587BCAB4F7A78532FA54AC6708444E}
```
