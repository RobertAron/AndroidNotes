# Fragments


#### 1. Fragment Setup

Create a place where the fragment will be placed in the parents layout.

`activity_mail.xml`

```xml
<LinearLayout>
    ...
    <FrameLayout
    android:id="@+id/fragment_placeholder"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_weight="1"/>
    ...
</LinearLayout>
```

#### 2. Create Fragment XML

`fragment_my.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MyFragment"
    android:background="@android:color/holo_blue_bright">
    <TextView
        android:id="@+id/fragment_text_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView" />
</LinearLayout>
```

#### 3. Setup Fragment Object.

1. Setup constants that will be used to cleanly place objects into `arguments`.
2. `onCreateView` is used to actually create a view that is generated. Take arguments and apply them as needed.
3. Setup a `compainion object`. This object is used as a factory to generate fragments cleanly. Place args into a bundle and set them as the fragments `arguments`

`MyFragment.kt`

```kotlin
private const val ARG_PARAM1 = "param1"
private const val ARG_PARAM2 = "param2"

class MyFragment : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?,
                              savedInstanceState: Bundle?): View? {
        // Inflate the layout for this fragment
        val fragView =  inflater.inflate(R.layout.fragment_my, container, false)
        arguments?.apply{
            val inputString = getString(ARG_PARAM1)
            val inputNumber = getInt(ARG_PARAM2)
            fragView.fragment_text_view.setText("$inputString then $inputNumber")
        }
        return fragView
    }

    companion object {
        fun newInstance(text:String,num:Int): MyFragment{
            val bundle = Bundle()
            bundle.putString(ARG_PARAM1,text)
            bundle.putInt(ARG_PARAM2,num)
            val myFragment = MyFragment()
            myFragment.arguments = bundle
            return myFragment
        }
    }
}
```


#### 4. Use the Fragment.

1. Instaniate the fragment.
2. Use the supportFragmentManager to replace the fragment holder.

`MainActivity.kt`

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val newFrag = MyFragment.newInstance("hello",10)
        supportFragmentManager.beginTransaction().replace(fragment_placeholder.id,newFrag).commit()
    }
}
```


## Accessing Elements

In order to access elements you will need to go through the view, unlike in normal activities.
setup might look something like this
```kotlin
private lateinit var fragButton:Button
private lateinit var fragEditText:EditText

override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?,
                            savedInstanceState: Bundle?): View? {
    // Inflate the layout for this fragment
    val fragView = inflater.inflate(R.layout.fragment_my, container, false)
    fragButton = fragView.fragment_button
    fragEditText = fragView.my_edit_text
    return fragView
}
```

## Using Sidenav

#### 1. Be careful of memeory leaks

Setup the nav_view to have one of the nav's selected. Setup the fragment when the state hasn't existed before. Only do this if it's not reloading the state.

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    ...
    if(savedInstanceState==null){
        supportFragmentManager.beginTransaction().replace(fragment_container.id, FragmentChat()).commit()
        nav_view.setCheckedItem(nav_menu_message)
    }
    ...
}
```

#### 2. Switch Fragments on button presses.

```kotlin
override fun onNavigationItemSelected(item: MenuItem): Boolean {
    when(item.itemId){
        nav_menu_message -> {supportFragmentManager.beginTransaction().replace(fragment_container.id, FragmentChat()).commit()}
        nav_menu_mail -> {supportFragmentManager.beginTransaction().replace(fragment_container.id, FragmentMail()).commit()}
        nav_menu_money -> {supportFragmentManager.beginTransaction().replace(fragment_container.id, FragmentMoney()).commit()}
    }
    drawer_layout.closeDrawer(Gravity.START)
    return true
}
```