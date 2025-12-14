---
title: AB Tests
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
#Overview

A/B tests is an extremely powerful and useful tool for product managers, tech leads or CTOs. A/B testing is a method of comparing two versions of a web page or app to each other and determine which one performs better. You can extend it to multivariate testing, where more than 2 variants can be compared. 

#Android

1. To enable connection to the Dashboard editor, in your Application class add the following line of code before `ActivityLifecycleCallback.register(this);` :
```CleverTapAPI.setUIEditorConnectionEnabled(true);``` 
[block:code]
{
  "codes": [
    {
      "code": "public class Application extends android.app.Application {\n    @Override\n    public void onCreate() {\n        CleverTapAPI.setUIEditorConnectionEnabled(true);//Set to false in production\n        CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG); //Set to OFF in production\n        ActivityLifecycleCallback.register(this);\n        super.onCreate();\n      //Your other Application code\n    }",
      "language": "java"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "Check that it is set to false in the production version."
}
[/block]
2. Use the following public methods for declaring Dynamic Variables:

[block:code]
{
  "codes": [
    {
      "code": "\ncleverTapAPI.registerBooleanVariable(\"booleanVar\"); //registers a Boolean variable\ncleverTapAPI.registerDoubleVariable(\"doubleVar\"); //registers a Double variable\ncleverTapAPI.registerIntegerVariable(\"integerVar\"); //registers a Integer variable\ncleverTapAPI.registerStringVariable(\"stringVar\"); //registers a String variable\n\ncleverTapAPI.registerListOfBooleanVariable(\"booleanList\"); //registers a List of Booleans\ncleverTapAPI.registerListOfDoubleVariable(\"doubleList\"); //registers a List of Doubles\ncleverTapAPI.registerListOfIntegerVariable(\"integerList\"); //registers a List of Integers\ncleverTapAPI.registerListOfStringVariable(\"stringList\"); //registers a List of Strings\n\ncleverTapAPI.registerMapOfBooleanVariable(\"booleanMap\"); //registers a Map of Booleans\ncleverTapAPI.registerMapOfDoubleVariable(\"doubleMap\"); //registers a Map of Doubles\ncleverTapAPI.registerMapOfIntegerVariable(\"integerMap\"); //registers a Map of Integers\ncleverTapAPI.registerMapOfStringVariable(\"stringMap\"); //registers a Map of Strings\n\n",
      "language": "java"
    }
  ]
}
[/block]
3. For retrieving values of the Dynamic Variables defined, use the following public methods:
[block:code]
{
  "codes": [
    {
      "code": "final Boolean boolVar = cleverTapAPI.getBooleanVariable(\"booleanVar\",null);\nfinal Double doublevar = cleverTapAPI.getDoubleVariable(\"doubleVar\",0.0);\nfinal Integer intVar = cleverTapAPI.getIntegerVariable(\"integerVar\",0);\nfinal String stringVar = cleverTapAPI.getStringVariable(\"stringVar\",\"default\");\n\nfinal List boolList = cleverTapAPI.getListOfBooleanVariable(\"booleanList\",null);\nfinal List doubList = cleverTapAPI.getListOfDoubleVariable(\"doubleList\",null);\nfinal List intList = cleverTapAPI.getListOfIntegerVariable(\"integerList\",null);\nfinal List strList = cleverTapAPI.getListOfStringVariable(\"stringList\",null);\n\nfinal Map<String,Boolean> booleanMap = cleverTapAPI.getMapOfBooleanVariable(\"booleanMap\",null);\nfinal Map<String,Double> doubleMap = cleverTapAPI.getMapOfDoubleVariable(\"doubleMap\",null);\nfinal Map<String,Integer> integerMap = cleverTapAPI.getMapOfIntegerVariable(\"integerMap\",null);\nfinal Map<String,String> stringMap = cleverTapAPI.getMapOfStringVariable(\"stringMap\",null);",
      "language": "java"
    }
  ]
}
[/block]
4. For using the [Visual editor](https://docs.clevertap.com/docs/visual-editor), follow the instructions on the Dashboard.

5. Your app may be missing a Resources class for your package due to your proguard configuration, or you may be using an applicationId in your build that is not the same as the package declared in your AndroidManifest.xml file. If you are using proguard, you can fix this issue by adding the following to your proguard configuration:
[block:code]
{
  "codes": [
    {
      "code": "-keep class **.R$* {<fields>;} \n\n",
      "language": "java"
    }
  ]
}
[/block]

 
If you're not using proguard, or if your proguard configuration already contains the directive above, you can add the following to your AndroidManifest.xml file to explicitly point the CleverTap SDK to the appropriate library for your resources class: 

[block:code]
{
  "codes": [
    {
      "code": "<meta-data \n\tandroid:name=\"CLEVERTAP_APP_PACKAGE\" \n\tandroid:value=\"YOUR_PACKAGE_NAME\" /> \n\n",
      "language": "xml"
    }
  ]
}
[/block]

where YOUR_PACKAGE_NAME is the same string that you use for the "package" attribute in your <manifest> tag.

6. To get a callback for the update of Dynamic variables or Visual experiments, implement the CTExperimentsListener and override the following method:

[block:code]
{
  "codes": [
    {
      "code": "@Override\npublic void CTExperimentsUpdated() {\n   //your code here\n}\n\n",
      "language": "java"
    }
  ]
}
[/block]
#iOS
 
1. Import the CleverTap  ABTesting Header. 


[block:code]
{
  "codes": [
    {
      "code": "#import <CleverTapSDK/CleverTap+ABTesting.h>\n   [CleverTap setUIEditorConnectionEnabled:YES];\n  \n  ",
      "language": "objectivec"
    },
    {
      "code": " CleverTap.setUIEditorConnectionEnabled(true)",
      "language": "swift"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "Check that it is set to false in the production version."
}
[/block]
2. Use the following public methods for declaring Dynamic Variables:

[block:code]
{
  "codes": [
    {
      "code": "\n#import <CleverTapSDK/CleverTap+ABTesting.h>// import the CleverTap ScreenAB Header\n\n\n [[CleverTap sharedInstance] registerBoolVariableWithName: @\"boolVar\"];\n [[CleverTap sharedInstance] registerDoubleVariableWithName: @\"doubleVar\"];\n [[CleverTap sharedInstance] registerIntegerVariableWithName: @\"intVar\"];\n [[CleverTap sharedInstance] registerStringVariableWithName: @\"stringVar\"];\n    \n [[CleverTap sharedInstance] registerArrayOfBoolVariableWithName: @\"arrayOfboolVar\"];\n [[CleverTap sharedInstance] registerArrayOfDoubleVariableWithName: @\"arrayOfdoubleVar\"];\n [[CleverTap sharedInstance] registerArrayOfIntegerVariableWithName: @\"arrayOfintVar\"];\n [[CleverTap sharedInstance] registerArrayOfStringVariableWithName: @\"arrayOfstringVar\"];\n   \n [[CleverTap sharedInstance] registerDictionaryOfBoolVariableWithName: @\"dictOfboolVar\"];\n [[CleverTap sharedInstance] registerDictionaryOfDoubleVariableWithName: @\"dictOfdoubleVar\"];\n [[CleverTap sharedInstance] registerDictionaryOfIntegerVariableWithName: @\"dictOfintVar\"];\n [[CleverTap sharedInstance] registerDictionaryOfStringVariableWithName: @\"dictOfstringVar\"];\n",
      "language": "objectivec"
    },
    {
      "code": "CleverTap.sharedInstance()?.registerBoolVariable(withName: \"boolVar\")\n CleverTap.sharedInstance()?.registerDoubleVariable(withName: \"doubleVar\")\n CleverTap.sharedInstance()?.registerIntegerVariable(withName: \"intVar\")\n CleverTap.sharedInstance()?.registerStringVariable(withName: \"stringVar\")\n CleverTap.sharedInstance()?.registerArrayOfBoolVariable(withName: \"arrayOfboolVar\")\n CleverTap.sharedInstance()?.registerArrayOfDoubleVariable(withName: \"arrayOfdoubleVar\")\n CleverTap.sharedInstance()?.registerArrayOfIntegerVariable(withName: \"arrayOfintVar\")\n CleverTap.sharedInstance()?.registerArrayOfStringVariable(withName: \"arrayOfstringVar\")\n CleverTap.sharedInstance()?.registerDictionaryOfBoolVariable(withName: \"dictOfboolVar\")\n CleverTap.sharedInstance()?.registerDictionaryOfDoubleVariable(withName: \"dictOfdoubleVar\")\n CleverTap.sharedInstance()?.registerDictionaryOfIntegerVariable(withName: \"dictOfintVar\")\n CleverTap.sharedInstance()?.registerDictionaryOfStringVariable(withName: \"dictOfstringVar\")\n",
      "language": "swift"
    }
  ]
}
[/block]
3. For retrieving values of the Dynamic Variables defined, use the following public methods:

[block:code]
{
  "codes": [
    {
      "code": "\n#import <CleverTapSDK/CleverTap+ABTesting.h>// import the CleverTap ScreenAB Header\n\n\n[[CleverTap sharedInstance] getStringVariableWithName:@\"stringVar\" defaultValue:@\"defaultStringValue\"];\n[[CleverTap sharedInstance] getBoolVariableWithName:@\"boolVar\" defaultValue:true];\n[[CleverTap sharedInstance] getDoubleVariableWithName:@\"doubleVar\" defaultValue:0.0];\n[[CleverTap sharedInstance] getIntegerVariableWithName:@\"intVar\" defaultValue:0];\n\n[[CleverTap sharedInstance] getArrayOfBoolVariableWithName:@\"arrayOfboolVar\" defaultValue:@[]];\n[[CleverTap sharedInstance] getArrayOfStringVariableWithName:@\"arrayOfstringVar\" defaultValue:@[@\"foo\", @\"shoes\"]];\n[[CleverTap sharedInstance] getArrayOfBoolVariableWithName:@\"arrayOfintVar\" defaultValue:@[@1,@1]];\n[[CleverTap sharedInstance] getArrayOfStringVariableWithName:@\"arrayOfdoubleVar\" defaultValue:@[]];\n\n[[CleverTap sharedInstance] getDictionaryOfBoolVariableWithName:@\"dictOfboolVar\" defaultValue:@{}];\n[[CleverTap sharedInstance] getDictionaryOfStringVariableWithName:@\"dictOfstringVar\" defaultValue:@{@\"foo\": @\"shoes\"}];\n[[CleverTap sharedInstance] getDictionaryOfBoolVariableWithName:@\"dictOfintVar\" defaultValue:@{@\"int\": @1}];\n[[CleverTap sharedInstance] getDictionaryOfStringVariableWithName:@\"dictOfdoubleVar\" defaultValue:@{}];\n\n",
      "language": "objectivec"
    },
    {
      "code": "CleverTap.sharedInstance()?.getBoolVariable(withName: \"boolVar\", defaultValue: true)  \n CleverTap.sharedInstance()?.getDoubleVariable(withName: \"doubleVar\", defaultValue: 0.0)\n CleverTap.sharedInstance()?.getIntegerVariable(withName: \"intVar\", defaultValue: 0)\n CleverTap.sharedInstance()?.getStringVariable(withName: \"stringVar\", defaultValue: \"defaultFooValue\")\n\n CleverTap.sharedInstance()?.getArrayOfBoolVariable(withName: \"arrayOfboolVar\", defaultValue: [true, false])\n CleverTap.sharedInstance()?.getArrayOfDoubleVariable(withName: \"arrayOfDoubleVar\", defaultValue: [1.1, 1.2])\n CleverTap.sharedInstance()?.getArrayOfIntegerVariable(withName: \"arrayOfIntegerVar\", defaultValue: [1, 2])\n CleverTap.sharedInstance()?.getArrayOfStringVariable(withName: \"arrayOfStringVar\", defaultValue: [\"foo\"])\n        \n CleverTap.sharedInstance()?.getDictionaryOfBoolVariable(withName: \"dictOfboolVar\", defaultValue: [\"key1\": true, \"key2\": false])\n CleverTap.sharedInstance()?.getDictionaryOfDoubleVariable(withName: \"dictOfdoubleVar\", defaultValue: [\"key1\": 1.1, \"key2\": 1.2])\n CleverTap.sharedInstance()?.getDictionaryOfIntegerVariable(withName: \"dictOfintVar\", defaultValue: [\"key1\": 1, \"key2\": 2])\n CleverTap.sharedInstance()?.getDictionaryOfStringVariable(withName: \"dictOfstringVar\", defaultValue: [\"key1\": \"a\", \"key2\": \"b\"])\n",
      "language": "swift"
    }
  ]
}
[/block]
4. For using the [Visual Editor](https://docs.clevertap.com/docs/visual-editor), follow the instructions on the Dashboard.
5. Use the following method to get a callback for the update of Dynamic variables or Visual experiments

[block:code]
{
  "codes": [
    {
      "code": "#import <CleverTapSDK/CleverTap+ABTesting.h>// import the CleverTap ScreenAB Header\n\n[[CleverTap sharedInstance] registerExperimentsUpdatedBlock:^{\n    ...  \n}];",
      "language": "objectivec"
    },
    {
      "code": "import CleverTapSDK\n\nCleverTap.sharedInstance()?.registerExperimentsUpdatedBlock {\n    ...\n} \n",
      "language": "swift"
    }
  ]
}
[/block]