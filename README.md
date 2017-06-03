# useful-kotlin-extensions

## goto Function

This is simple an intuitive way to start a new activity  
Place this code in a helper file

      fun Context.goto(javaClass: Class<*>) : Intent{
      val intent : Intent = Intent(this, javaClass)
      return intent
      }

      inline fun Intent.add (body: Intent.() -> Unit) { body() }

      fun Intent.start(context: Context){
      context.startActivity(this)
      }

and then you can do something like this

     context.goto(MyClass::class.java).add {
          addFlags(Intent.FLAG_ACTIVITY_NO_HISTORY)
          putExtra("ruleId", ruleId)
          putExtra("type", "Example")
          start(context)
     }

if you don't need to add flags or extras you can just do this  

      context.goto(MyClass::class.java).start(context)

## toPrefs function
This is a very simple way to put something into shared Preferences  
I only have the int and String versions here but I'm sure you can work out how to get the other primitive types to work  
Place this code in a helper file  

    fun Int.toPrefs(key: Int, context: Context){
        val s: String = context.resources.getString(key)
        val prefs = getDefaultSharedPreferences(context)
        prefs.edit().putInt(s, this).apply()
    }

    fun String.toPrefs(key: Int, context: Context){
        val s: String = context.resources.getString(key)
        val prefs = getDefaultSharedPreferences(context)
        prefs.edit().putString(s, this).apply()
    }

Then all you need to do to put something into shared Preferences is:

       // for an integer
       5.toPrefs(R.string.key, context)

       // or if you have a var
       var myNumber = 0
       myNumber.toPrefs(R.string.key, context)

       // for a string
       "My string".toPrefs(R.string.key, context)
       
       // With a string var
       var myString = "My string"
       myString.toPrefs(R.string.key, context)

## toast Function
A simple way to show toasts in Kotlin

      fun String.toast(context: Context)= Toast.makeText(context, this, Toast.LENGTH_SHORT).show()
      fun String.toastLong(context: Context) = Toast.makeText(context, this, Toast.LENGTH_LONG).show()
      
Then to show a toast 

      "This is my toast".toast(context)
      
      val myString = "This is my toast"
      myString.toast(context)
      
      // or to show a long toast
      myString.toastLong(context)
      
## forWith
This is a kotlin for loop with a with function

     inline fun <T> forWith(items: Iterable<T>, body: T.() -> Unit){
         for(item in items){
         item.body()
         }
     }

With this function you can do something like this:

      forWith (people) {
          hairColor = "blond"
          height = 5.12
          weight = 140
          age = 5
      }
      
instead of this

      for (person in people){
          person.hairColor = "blond"
          person.height = 5.12
          person.weight = 140
          person.age = 5       
      }
      
