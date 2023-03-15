# Saving files

The Android app can save files to the device's storage. This is useful for saving data from sensors or other sources. The app can also load files from the device's storage. This is useful for loading data from a file to be processed by the app.

## Saving files

To be able to save files, you need to add the following permissions to the `AndroidManifest.xml` file:

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

And when you start the app, you need to request the permission:

```java

    if(Build.VERSION.SDK_INT > Build.VERSION_CODES.M
            && checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED) {
        requestPermissions(new String[] {android.Manifest.permission.WRITE_EXTERNAL_STORAGE}, 101);
    }

```

Dont forget import the `android.Manifest` package. Any unimported packages will be highlighted in red. You can import the package by clicking on the red highlighted package name and selecting the option to import the package.

## Saving jason files

To save a json file, you save the json object to a file as any other text file. The following example shows how to save a json file:

```java

JSONObject mJsonObject = new JSONObject();
mJsonObject.put("name", "John"); // add a key value pair to the json object

File file = new File(path); // path is the path to the file"
output = new BufferedWriter(new FileWriter(file));
output.write(mJsonObject.toString());
output.close();

```
## Chosing the save location

I recomend using a libary such as [FilePicker](https://github.com/RMartinPer/File-Picker).
This wiil allow you to choose the save location and the file name. The following example shows how to use the FilePicker library to save a file:

```java

    public void ChooseSaveFolder(View view){

        DialogProperties properties = new DialogProperties(true);
        properties.setSelectionType(DialogConfigs.DIR_SELECT);
        FilePickerDialog dialog = new FilePickerDialog(MainActivity.this, properties);
        dialog.setTitle("Select a folder");
        dialog.setDialogSelectionListener(new DialogSelectionListener() {
            @Override
            public void onSelectedFilePaths(String[] files) {
                //files is the array of the paths of files selected by the Application User.
                Log.i("Files", "Selected: " + files[0]);
                SelectedFilePath = files[0];
                Toast.makeText(getApplicationContext(), "Selected folder path: " + SelectedFilePath, Toast.LENGTH_LONG).show();
            }
        });
        dialog.show();
    }

```

For more details on how to use the FilePicker library, see the [FilePicker GitHub page](https://github.com/RMartinPer/File-Picker).