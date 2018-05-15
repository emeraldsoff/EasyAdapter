# EasyAdapter (Support only with DataBinding)

- Removes Boilerplate code to create adapter and holder.
- You can filter adapter without coding much.
- You wil have load more feature with loading at bottom.
- It has swipe to action functionality.
- View Events callbacks (ClickEvent,CheckChangeEvent)
- and many more..

Download
--------

Grab via Maven:
```xml
<dependency>
  <groupId>com.dc.easyadapter</groupId>
  <artifactId>easyadapter</artifactId>
  <version>1.0</version>
  <type>pom</type>
</dependency>
```
or Gradle:
```groovy
implementation 'com.dc.easyadapter:easyadapter:1.0'
```

To enable data binding
-------------------------

inside app build.gradle
```groovy
android {
    dataBinding {
        enabled = true
    }
}
```

For Kotlin also add
 ```groovy
 dependencies{
        kapt 'com.android.databinding:compiler:3.1.2'
}

apply plugin: 'kotlin-kapt' //Top at build.gradle
```

## Adapter Creation

``` kotlin
class CategoryAdapter() :EasyAdapter<Category, InflaterCategoryBinding>(R.layout.inflater_category) {

    override fun onBind(binding: InflaterCategoryBinding, model: Category) {
        binding.apply {
            tvName.text = model.name
            cbCategory.isChecked = model.isSelected
        }
    }
}
```

## Usage

#### To Handle recycler View item Events 

``` kotlin
//Override in Adapter
override fun onCreatingHolder(binding: InflaterCategoryBinding, baseHolder: BaseHolder) {
        super.onCreatingHolder(binding, baseHolder)
        binding.root.setOnClickListener(baseHolder.clickListener)
    }
```

``` kotlin
adapter.setRecyclerViewItemClick { itemView, model -> 
//Perform Operation here 
}
```

#### Filter (Search,etc..)
``` kotlin
adapter.performFilter(text, object :EasyAdapter.OnFilter<Category>{
                    override fun onFilterApply(text: String, model: Category): Boolean {
                        if (model.name?.toLowerCase()?.contains(text.toLowerCase())!!) {
                            return true //Return True if you want to include this model in this text search
                        }
                        return false //It will not include model if you return false
                    }

                    override fun onResult(data: java.util.ArrayList<Category>?) {
                        //Filtered List to do any operation
                    }
                })

```

#### Load More
``` kotlin
adapter.enableLoadMore(binding.recyclerView, EasyAdapter.OnLoadMoreListener {
            if (paging != -1) {
                requestLoadMore() //Your Method
                return@OnLoadMoreListener true // Returns True if you have more data
            }
            return@OnLoadMoreListener false // Return false if you don't have more data
        })

```

Pro Tips

1. Use tools attribute for previewing Layout, so you don't need to always run application

 - inside recyclerview
  
  ```xml
 tools:listitem="@layout/inflater_category"
 tools:itemCount="5"
 tools:orientation="horizontal"
 app:layoutManager="android.support.v7.widget.GridLayoutManager"

```
 
 - inside any layout
 
 ```xml
 tools:text="Sample Text"
 tools:visibility="VISIBLE"
 tools:background="@color/colorPrimary"
```
 
 - you can also use android predefine sample data by
 
```xml
 tools:text="@tools:sample/cities,first_names,us_phones,lorem,lorem/random"
 tools:background="@tools:sample/backgrounds/scenic"
 tools:src="@tools/avatars"
```
 
 - you can also make your own sample data
 
    To create your fake/sample data folder,
    just right click on the “app” folder then “new > Sample Data directory” <br />
    create new file with "filename" and write each text by new lines
 
    file contains -
    
    Air Conditioning Mechanic <br />
    Antenner Installer <br />
    HVAC Mechanic <br />
    Electrician <br />
    
    so it will randomly pick names and display in layout by

```xml
tools:text="@sample/filename" 
```


License
=======

    Copyright 2013 DC, Inc.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
