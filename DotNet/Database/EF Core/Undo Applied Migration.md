



1- Get the list of migrations from Database

```sql
[__EFMigrationsHistory]
```


2- Choose the migration you want to restore too.
```
20260824120847_AddPublishDateTimeToBlogs
20260819082851_Change-TeacherProfileVisitsModelAndCourseVisitsModel
20260818085720_Add-TeacherProfileVisitsModelAndCourseVisitsModel
20260811083719_RemoveTeacherProfileCompletionDefault
20260811080955_AddTeacherProfileCompletion
20260729133816_AddInstantClassQuickRoomSchema
20260715145853_AddBuyServiceOnlineClassCapacity
```


3- Run this command in NuGet Package Manager Console:
```shell
Update-Database -Migration 20260819082851_Change-TeacherProfileVisitsModelAndCourseVisitsModel
```