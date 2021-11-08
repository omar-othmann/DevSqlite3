# DevSqlite3
A python package to create sqlite3 database with python class.

## install by PyPI:

```
pip3 install DevSqlite3
or
python -m pip install DevSqlite3
```

if you have old version:
```
pip3 install --upgrade DevSqlite3
python -m pip install --upgrade DevSqlite3
```

## setup sqlite3 database

Note: You can add field and remove it from your class, it's will automatically removed or added from sqlite3 database

also you can use:

1- dropColumnNotExists=False or True # default True
2- addColumnNotExists=False or True # default True

that's mean when your remove some fields from your class, it's will removed from database table also when adding fields to your class, it's will added to database

to use:
```
@Database('DatabaseFileName', path='folderName', dropColumnNotExists=False, addColumnNotExists=False)
```

Note: path, dropColumnNotExists, addColumnNotExists is not requires.

create sqlite3 database:

```
from DevSqlite3.core import Database, Table


@Database('DatabaseFileName', path='folderName')
class Users(Table):
	id = Table.integerField(primary=True, null=False) # id integer primary key not null
	username = Table.stringField() # username text, if you want to add not null Table.stringField(null=False)
	password = Table.stringField()
	join_time = Table.dateField()
	nicks = Table.listField()
	owned_item = table.dictField()
	# if you want to ignore column, make variable name starts with __, for example
	__ignore__ = "any"
	
```

you can also add custom function like Dao in room database:

```
@Database('DatabaseFileName', path='folderName')
class Users(Table):
	id = Table.integerField(primary=True, null=False) # id integer primary key not null
	username = Table.stringField() # username text, if you want to add not null Table.stringField(null=False)
	password = Table.stringField()
	join_time = Table.dateField()
	nicks = Table.listField()
	owned_item = table.dictField()
	
	def getUsers(self):
        return self.execute("select * from User").all()  # return list of class Users if detected else empty list

    def getUserById(self, i):
        return self.execute("select * from User where id=:id",
                            args={"id": i}).first()  # return Users class if detected else None

    def updatePassword(self, username, password):
        return self.execute("update User set password=:password where username=:username",
                            args={"username": username, "password": password}).run()  # return None if update or last row id if insert
```

## getting data from database
```
u = Users()
users = u.getUsers()
for user in users:
	print(user.id, user.username, user.join_time)
	
	# update all
	
	user.password = "123"
	user.save()

user = u.getUserById(1)
if user:
	# delete
	user.delete()
	
	# or update
	user.password = "123"
	user.nicks = ["1", "2", "3"]
	user.owned_item = {"colors": ["red", "white", "etc"]}
	user.save()
```

## insert data to database
```
from datetime import datetime

u = Users()
u.username = "omar.othman"
u.password = "password"
u.join_time = datetime.now()
u.save()

```

## are you noob with sqlite commands? don't worry we can help you
```
u = Users()
find = u.where('username').equals('omar.othman').andWhere('password').equals('password').first() # select * from Users where username='omar.othman' and password='password'
if find:
	find.password = "newPassword"
	find.save() # update
else:
	u.username = "omar.othman"
	u.password = "newPassword"
	u.save() # insert
```

# aslo you can add more tables to database

```
@Database('DatabaseFileName', path='folderName') # The database file name must be the same as the other name, or a new one will be created
class AnotherTableName(Table):
	# etc ...
```

## about where

functions | paramerets | about
----------- ----------- | -----------
orWhere | column name | sqlite 'or'
andWhere | column name | sqlite 'and'
equals | value | where column row equals value
notEquals | value | where column row not equals value
like | value, before=True, after=True | where column row like value, before it's mean %value and after value% when before and after is True: %value%
notLike | value, before=True, after=True | where column row not like value, before it's mean %value and after value% when before and after is True: %value%
orderBy | column name, stuff="asc", limit=0 | sort by column name, stuff can be "asc" or "desc", limit 0 is unlimited
all | empty | to get list of class or empty list if not found
first | empty | to get first result as class or None if not found
run | empty | sent your sql command to database if command is insert will return lastrowid else return None


## python version requires >=2


## about me
Name: Omar Othman
Birth: 05.01.1995 / DD.MM.YYYY
Mother country: Syria
Live: Germany

Donate: https://paypal.me/nxdev


# update info
1- remove __superclass__ from custom class table
2- change 'get_all' to 'all'
3- change 'get_first' to 'first'
4- added 'run' function


# next update?
* support MySql database!!!








		