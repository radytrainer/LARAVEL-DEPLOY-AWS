# Laravel DEPLOY ON AWS

### STEP 1: CLONE PROJECT 
we need to go <code> /var/www/html </code> and then use <code> git clone your_reposity </code> to clone the project.

> cd <code>/var/www/html</code> <br/>
> git clone <code>your_reposity</code> <br/>

### STEP 2: <code>ENVIROMENT VARIABLE </code> FILE
In Laravel when we push the project to GitHub we ignore the <code>.env </code> file but we still have <code>.env.example</code> file.

You just copy <code>.env.example</code> and rename to <code>.env</code> by following command in linux:

> cp <code>.env.example</code><code> .env</code>

### STEP 3: <code>CREATE DATABASE</code>
You need to create the database and config in <code>.env</code> file on <code>DB_DATABASE= your_db_name</code> example:
> DB_DATABASE=<code>example_db</code> <br/>
> DB_USERNAME=<code>root</code> <br/>
> DB_PASSWORD=<code>123</code> <br/>

### STEP 4: <code>LARAVEL KEY GENERATOR</code>
When we copy <code>.env.example</code> file and rename to <code>.env</code> file in <code>APP_KEY=</code> is empty so you cannot run your Laravel project because in security purpose.

So to fix this issue we need to run the following command:
> php artisan key:generate

### STEP 5: <code>MIGRATE TABLE TO DATABASE</code>
Following the step above now we can run migrations command to migrate our table to database.
> php artisan migrate

For some reason with your Database instance Maria-DB version you might be meet the error like <code><i> Syntax error or access violation: 1071 Specified key was too long; max key length is 767 bytes </i> </code>. To fix this issue you need to add some code on <code> AppServiceProvider.php</code>.

> <code>app/Providers/AppServiceProvider.php</code> <br />
```
use Illuminate\Support\Facades\Schema; // add this line
public function boot()
{
    Schema::defaultStringLength(191); // add this line
}
```
After that you need to delete all your table in your database and then run your migration again: <code> php artisan migrate </code>

### STEP 6: GIVE PERMISSIONS TO <code>storage</code> folder.
When we are on Server we don't have permission to access the storage folder so we need to give the permission to user by following the command:

> sudo chmod 777 -R storage

### STEP 7: <code> ALLOW OVERRRIDE </code> 
After we finish step 6 we still cannot access the path on URL or link to other pages we need to to allow override in <code>httpd.conf</code> file. 

>sudo vi <code>/etc/httpd/conf/httpd.conf</code> <br/>

Press <code> i </code> on keyboard to insert or edit file and then scroll down to find:
```
# Purther relax access to the default document root:
<Directory "/var/www/html">
................................................................
................................................................
AllowOverride None 
```
> AllowOverride <code> None </code> 

Update <code>None</code> to <code>All</code>
> AllowOverride <code> All </code> 

After that we need to restart the server by running the following command:
>sudo <code>systemctl restart httpd.service</code>

### STEP 8: 
You might be have a problem with images when upload and cannot display this problem because of <code>systemlink</code> so you need to put your image in <code>public</code> folder and also you need to run the following command:
> php artisan storage:link 

#### <code>Copyright by WEP Training Team @PNC</code>