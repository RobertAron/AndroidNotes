# Notes on how to implement the Recycler View

The recycler view is used like an Object Pool. It is used to save memory.

There are three main parts

- `Layout` - XML that is how each item will be displayed.
- `ViewHolder` - Caches view objects to save memeory
- `Adapter`
  - creates new items in the form of `ViewHolders`
  - populates the `ViewHolders` with data
  - interfaces to the data.

#### 1. Import the dependency

`build.gradle (Module: app)`

```gradle
dependencies {
  ...
  implementation 'com.android.support:recyclerview-v7:27.1.1'
  ...
}
```


#### 2. Create the Recycler view that will filled. If you're fragment is a single recycler, it can just be top level.

`my_recycler_view.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.RecyclerView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/id_recycler_reference"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:layout_editor_absoluteX="8dp"
    tools:layout_editor_absoluteY="8dp" />
```


#### 3. Create a `Layout`. This displays each 'row'

`my_simple_row.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="10dp">

    <TextView
        android:id="@+id/recycler_position_text"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="POSITION HERE" />

    <TextView
        android:id="@+id/recycler_class_data_text"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="DATA HERE" />
</LinearLayout>
```

#### 4. Create a class to hold your data. You could also just use a String.

`Animal.kt`

```kotlin
data class Animal(val animalName:String)
```
#### 5. Create the `Adapter`

`AnimalAdapter.kt`

```kotlin
class AnimalAdapter : RecyclerView.Adapter<MyViewHolder>(){
    val items = mutableListOf<Animal>()
    override fun getItemCount(): Int {
        return items.size
    }
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        return MyViewHolder(LayoutInflater.from(parent.context).inflate(R.layout.my_simple_row, parent, false))
    }
    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        holder.bind(items[position],position)
    }
}
// A single view that will be reused as things are scrolled around.
// Implement how to update it as it moves around.
class MyViewHolder (view: View) : RecyclerView.ViewHolder(view){
    val positionText = view.recycler_position_text
    val classDataText = view.recycler_class_data_text

    // write whatever kind of functions to fix the views.
    fun bind(animal:Animal,position:Int){
        positionText.text = position.toString()
        classDataText.text = animal.animalName
    }
}
```



#### 6. Set it up to display!

`MainActivity.kt`

```kotlin
class MainActivity : AppCompatActivity() {
    val myAdapter = AnimalAdapter()

    fun loopAddItems(){
        Timer().schedule(500){
            myAdapter.items.add(Animal("Item Added"))
            val newItemPosition = myAdapter.items.size - 1
            runOnUiThread({myAdapter.notifyItemInserted(newItemPosition)})
            loopAddItems()
        }
    }
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(my_recycler_view)
        id_recycler_reference.adapter = myAdapter
        id_recycler_reference.layoutManager = LinearLayoutManager(this)
        loopAddItems()
    }
}
```

careful on this import

```kotlin
import kotlinx.android.synthetic.main.my_recycler_view.id_recycler_reference
//import corp.aron.robert.howtorecycle.R.id.id_recycler_reference <-- NOT THIS ONE
```

![recycler example](https://github.com/RobertAron/AndroidNotes/blob/master/res/recycler_example.gif)


### RecyclerAdapterTemplate

```kotlin
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME}#end

import android.support.v7.widget.RecyclerView
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup

class ${NAME} : RecyclerView.Adapter<${Viewholder_Class}>(){
    var items = emptyList<${Item_Class}>()
    override fun getItemCount(): Int {
        return items.size
    }
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ${Viewholder_Class} {
        return ${Viewholder_Class}(LayoutInflater.from(parent.context).inflate(/*TODO:: R.Layout.Reference*/, parent, false))
    }
    override fun onBindViewHolder(holder: ${Viewholder_Class}, position: Int) {
        holder.bind(items[position])
    }
    fun addItems(newItem:${Item_Class}){
        // TODO:: Implement data modifiers
        // TODO:: Avoid notifyDataSetChanged()
    }
}

class ${Viewholder_Class}(view: View) : RecyclerView.ViewHolder(view){
    // val textView = view.textView
    
    fun bind(item:${Item_Class}){
        // textView.text = item.propery.toString()
    }
}
```