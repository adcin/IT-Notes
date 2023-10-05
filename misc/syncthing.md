# Syncthing description

</br>

Source: `man syncthing`  
>Syncthing  lets you synchronize your files bidirectionally across multiple devices. This means the creation, modification or deletion of files on one machine will automatically be replicated to your other devices.

--------------

</br>

# Troubleshooting

</br>

## Error: folder marker missing

This happens when the `.stfolder` file is missing in the folder you want to sync. The solution is simple - create an empty file named `.stfolder`:  
```shell
touch .stfolder
```