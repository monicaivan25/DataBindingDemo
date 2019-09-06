# DataBindingDemo
Kotlin data binding PoC
## View Binding
#### 1. Enable data binding in your build.gradle file in the app module inside the android section:
```gradle
dataBinding {
  enabled = true
}
```
#### 2. Wrap all views in activity_main.xml into a ```<layout>``` tag, and move the namespace declarations into the the ```<layout>``` tag.
```xml
<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        tools:context=".MainActivity">

        <!-- layout elements here -->
</layout>
```
#### 3. In MainActivity, create a binding object:
```kotlin
private lateinit var binding: ActivityMainBinding
```
#### 4. In onCreate, use DataBindingUtil to set the content view:
```kotlin
binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
```
#### 5. Use the binding object to replace all calls to findViewById, for example:
```kotlin
binding.nicknameEditText.visibility = View.GONE
binding.doneButton.visibility = View.GONE
```
or
```kotlin
binding.apply{
    nicknameEditText.visibility = View.GONE
    doneButton.visibility = View.GONE
}
```

## Data Binding
Do everything from view binding, and then follow the steps:
#### 1. Create a data class MyName for the name and nickname.
```kotlin
data class MyName(var name: String = "", var nickname: String = "")
```
#### 2.Add a <data> block to activity_main.xml. The data block goes inside the layout tag but before the view tags. Inside the data block, add a variable for the MyName class.
```xml
<data>
<!-- Declare a variable by specifying a name and a data type. -->
<!-- Use fully qualified name for the type. -->
<variable
    name="myName"
    type="com.example.android.aboutme.MyName" />
</data>
```
#### 3. In name_text, nickname_edit, and nickname_text, replace the references to string text resources with references to the variables, for example:
```xml
android:text="@={myName.name}"
```
#### 4. In MainActivity, create an instance of MyName.
```kotlin
private val myName: MyName = MyName("Monica Ivan")
```
#### 5. And in onCreate(), set binding.myName to it.
```kotlin
binding.myName = myName
````
#### 6. In the doneButton onClickListener, set the value of nickname in myName, call invalidateAll(), and the data should show in your views.
```kotlin
myName?.nickname = nicknameEdit.text.toString()
// Invalidate all binding expressions and request a new rebind to refresh UI
invalidateAll()
```
