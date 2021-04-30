import datetime
import psycopg2
# connect to the database of choice
conn = psycopg2.connect(dbname ='Brothers_Data', user='postgres', password='secrets')

#cur is the module that allows for sql commends to operate on the database
cur = conn.cursor()

# you are now in the database and can make commands to alter, insert, query tables using the execute function
cur.execute('SELECT * from employees')

# This is the method to display data from the table. Since the output is a list, readability should be augmented easily
result = cur.fetchall()
print(result)

# Example of insert statement
cur.execute(
    '''INSERT into employees
    (name, join_date) 
    values (%s, %s);''',
    ('Lucas Powers', datetime.date(2018, 7, 2))
)

# Check that the table has been changed as expected
cur.execute('SELECT * from employees')
result = cur.fetchall()
print(result)

# Something like this will make the output more readable:
#for i in result:
#    print(i)
    
# make any changes permanent
#conn.commit()

# Let's create a throwaway table
cur.execute('CREATE TABLE guests(id serial PRIMARY KEY, name varchar(40) NOT NULL, age int NOT NULL);')

# We can start to integrate python OOP
name = input('type your name, please')
try:
    age = int(input('How old are you'))
    cur.execute('INSERT INTO guests(name, age) values(%s, %s);', (name,age))
except ValueError:
    print('You didn\'t type your age nicely')
cur.execute('SELECT * from guests;')
print(cur.fetchall())

# Let's drop the table to declutter things.
cur.execute('DROP TABLE guests;')

# Now we can try to fill a whole table with a for loop.
ct = pd.read_csv('city_temperature.csv')
counter = 0
for row in ct.itertuples():
# We don't need to go overboard and do the whole dataset; how about 50 rows
    while counter < 50:
        cur.execute('''INSERT INTO city_temperatures(region, country, city, month, day, year, avgtemperature)
        values (%s, %s, %s, %s, %s, %s, %s);''', (row.Region, row.Country, row.City,
                                                      row.Month, row.Day, row.Year, row.AvgTemperature))
        counter+=1

cur.execute('SELECT * from city_temperatures;')
# We can display an arbitrary amount of rows, like .head() in pandas
result_set = cur.fetchmany(30)
# And lets Display this nicely again.
for row in result_set:
    print(row)

# We can finish by dropping this table as well.
cur.execute('DROP TABLE city_temperatures;')


# always sever connection to the server
cur.close()
conn.close()
