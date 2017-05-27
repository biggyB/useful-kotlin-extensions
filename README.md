# useful-kotlin-extensions

## goto Function

This is simple an intuitive way to start a new activity  
Place this code in a helper file

      fun Context.goto(javaClass: Class<*>) : Intent{
      val intent : Intent = Intent(this, javaClass)
      return intent
      }

      inline fun Intent.with (body: Intent.() -> Unit) { body() }

      fun Intent.start(context: Context){
      context.startActivity(this)
      }

and then you can do something like this

     context.goto(MyClass::class.java).with {
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
