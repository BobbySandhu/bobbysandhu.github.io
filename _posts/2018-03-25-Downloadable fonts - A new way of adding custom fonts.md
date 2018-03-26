### Downloadable fonts

##### Downloadable Fonts:

In *Google I/O 2017* Google announced [support library 26](https://developer.android.com/about/versions/oreo/android-8.0.html) with support API for managing fonts in the Android App as a resource. This introduction of API made using custom fonts in android app a lot easier than before and is supported from version Oreo (Api level 26) till Api level 14.

This has made using thousands of Google Fonts in our app as easy as adding a resource and that even is done by Android Studio itself.

**So, what is Downloadable Fonts…?**

> Downloadable fonts is a way to request (use) fonts from provider
> application and not bundling them into the application(apk).

*Explanation with a scenario,* earlier if we want to use a font that we like into our application, we use to put that font in ‘*assets*’ folder and calling that font all over the app activities and fragments. Problem with this was, if the font size is larger it will directly impact the app size.

Apart from that if we have 10 different applications installed in our cell phone and using the same font in them then android system in our phone will have multiple copies of that font and unnecessary taking up the disk space 10 times. However it was an easy solution.

In order to overcome this problem of managing and using custom fonts, Google has introduced this Downloadable Fonts feature which will solve this problem by introducing *support library 26 with Android Oreo (8.0)*. This library is backward compatible till Android api level 14, which means using this in our app will work on older devices as well.

**Benefits of using Downloadable Fonts:**

As per Google Developers site here are the benefits of using it:

-   It reduces the APK size (no need to bundle .ttf files in the app)

-   Increases the app installation success rate

-   Improves the overall system health as multiple APKs can share the same font through a provider. This saves users cellular data, phone memory, and disk space. In this model, the

-   font is fetched over the network when needed.

**How it works?**

Requesting a custom font is handled by ‘`Font Provider`’ application. All applications in our phone that use downloadable fonts pass their requests via `FontsContractCompat`. It then communicates with the requested font provider. A font provider is an application that takes care of fetching and caching the appropriate fonts.

Illustration by Android Developer website for explaining it:

![](https://lh5.googleusercontent.com/160_YsvWWWJeIZ17fbji_m6kNgHt9V2L0PUjo1MMgLVVw0j-FUaGSGriMMkGDYS7VZZBQUtuJi6rzDk0-MQaMcXSd-KqMneDr8q2rtWY0_mEtkcT1tiyc00jp25AqCPw581xW3WV)

**Why can’t we use Font straightaway in Android?**

> Font files are executable and android system ensures and provides a
> secure way to use it so that no malicious code is ever executed by it.

**How to use Downloadable fonts in our app…?**

There are three ways to use it in our Application:

1.  via Android Studio and Google Play Services

2.  Programmatically in class file

3.  Via the support library


We are using Android Studio to generate the required files for us, and used the Fonts in the XML file feature available in the Support library to apply the downloaded fonts.

The reason we decided to do it via XML is because then we can declare the required fonts in our app’s manifest file, which allows the framework to download them *ahead of time*(fetching them before they actually be rendered). If we do it programmatically then we can request for fonts only after the app is launched, which can cause a delay in the first layout rendering time. Also, *it is less work do it via XML!*

**Step1:** *Importing the api*

    implementation "com.android.support:support-compat:27.0.2"

**Step2:**  *Selecting TextView for applying font*

Go to your layout XML and hit design mode in the editor and select TextView from the view hierarchy.

<img src="/images/step2.png" alt="roomdb1" style="width: 300px;"/>

**Step3:**  *Selecting fontFamily*

Select `fontFamily` in the graphical layout attribute section on the right hand side and choose `More Fonts…` in the bottom of the popup list.

<img src="/images/step3.png" alt="roomdb1" style="width: 300px;"/>

**Step4:**  *Choose Font*

After clicking more fonts, a new window will popup and here you can select the custom font which you want.

Select the font name you want and on right side of the window don’t forget to select `create downloadable font` radio button.

<img src="/images/step4.png" alt="roomdb1" style="width: 300px;"/>

Here most of your work is done. This step would result in 3 files.

`<your-font-name>.xml (e.g. Aladin.xml)`, `font_certs.xml` and `preloaded_fonts.xml`.

**Aladin.xml:**

This file contains the various different attribute that are needed to load the typeface from google. This file looks something like:

    <?xml version="1.0" encoding="utf-8"?>  
    <font-family xmlns:app="http://schemas.android.com/apk/res-auto"  
    app:fontProviderAuthority="com.google.android.gms.fonts"  
    app:fontProviderPackage="com.google.android.gms"  
    app:fontProviderQuery="Aladin"  
    app:fontProviderCerts="@array/com\_google\_android\_gms\_fonts_certs">  
    </font-family>

**font_certs.xml:**

This file is used to verify the identity of font provider and is called a font certificate file. All the information for verification purposes is mention here to avoid any font from any unknown source. Android Studio will auto-generate this file. It looks something like:

    <?xml version="1.0" encoding="utf-8"?>  
    <resources>  
    <array name="com\_google\_android\_gms\_fonts_certs">  
    <item>@array/com\_google\_android\_gms\_fonts\_certs\_dev</item>  
    <item>@array/com\_google\_android\_gms\_fonts\_certs\_prod</item>  
    </array>  
    <string-array name="com\_google\_android\_gms\_fonts\_certs\_dev">  
    <item>  
    <!\-\- string cert -->  
    </item>  
    </string-array>  
    <string-array name="com\_google\_android\_gms\_fonts\_certs\_prod">  
    <item>  
    <!\-\- string cert-->  
    </item>  
    </string-array>  
    </resources>

**Preloaded_fonts.xml**

This file is used to pre-load the font in app before it is actually used and is referenced in `AndroidManifest.xml.`

    <?xml version="1.0" encoding="utf-8"?>  
    <resources>  
    <array name="preloaded_fonts" translatable="false">  
    <item>@font/lato</item>  
    <item>@font/lato_bold</item>  
    </array>  
    </resources>

An entry of this file in Manifest:

    <meta-data  
    android:name="preloaded_fonts"  
    android:resource="@array/preloaded_fonts"/>

**Step 5:** *Adding font for a TextView*

Although, all these tasks are done by Android Studio automatically but you must know the things that are being done under the hood.

Here you will see a new attribute in the Textview

    android:fontFamily="@font/aladin

There you go!!

In this way you can use custom fonts for your app. However, there are certain situations where you need a font to be applied at runtime, there you can use it programatically. The links for that are added down below.

**Final Output:**

<img src="/images/df_output.png" alt="roomdb1" style="width: 300px;"/>

***Thanks for reading !!***

**Reference:** https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html
