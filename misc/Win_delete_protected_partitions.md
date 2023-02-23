# Delete hidden partition on USB drive via Diskpart command

Diskpart is a text-mode command interpreter, and it is able to help you create, delete or format partition. To remove hidden partition on hard drive, first of all, press “Windows+R” simultaneously to open "Run" window and type “cmd” and press "Enter", then in the Command Prompt window, type the following commands in sequence and hit on “Enter” after each one.

```shell
diskpart
```

```shell
list disk
```

```shell
select disk #
```
- “#” is the number of the target USB disk.

```shell
list partition
```

```shell
select partition #
```
- “#” is the number of the partition you want to delete.

```shell
delete partition
```

Sometimes, the delete operation failed to work with displaying an error message “Cannot delete a protected partition without the force protected parameter set”, then you need to type:

```shell
delete partition override
```
