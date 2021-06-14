Foreign key declarations in MySQL
===============

Posted: 2014-04-26 18:02:19
-------------------------

In my few years working closely with MySQL, I've found something surprising about how foreign keys are declared.&#160;For the longest time, I've been used to declaring foreign keys like so:

[code lang="sql"]create table foo (
foo_id int(11) primary key auto_increment,
-- ...
);

create table bar (
bar_id int(11) primary key auto_increment,
foo_id int(11) not null references foo(foo_id),
-- ...
);[/code]

After a while, I started noticing that foreign key constraints were not being enforced like I thought they would be. When I looked at the <code>SHOW CREATE TABLE</code> statement for these tables, I found something surprising: the foreign keys were not created! After a bit of frustration, I took to the internet, and found the following quote in <a href="https://dev.mysql.com/doc/refman/5.6/en/create-table-foreign-keys.html">MySQL's documentation</a>:

<blockquote>MySQL does not recognize or support "inline REFERENCES specifications&#8221; (as defined in the SQL standard) where the references are defined as part of the column specification. MySQL accepts REFERENCES clauses <strong>only</strong> when specified as part of a separate FOREIGN KEY specification.</blockquote>

(emphasis mine)

What's worse, when you try to use an "inline REFERENCES" as above, MySQL will silently ignore it! I hunted around a bit to see if I could find any way to make this syntax a fatal error, but there doesn't seem to be. So anyway, you need to create foreign keys the "long way" in MySQL:

[code lang="sql"]create table bar (
bar_id int(11) primary key auto_increment,
foo_id int(11) not null,
-- ...
foreign key bar_foo_id (foo_id) references foo(foo_id)
);[/code]

It's frustrating that the inline REFERENCES is just ignored instead of treated as a syntax error - it fools you into thinking that your DDL was fully processed, but you don't get so much as a warning that it was not processed 100% as you might expect. Does anyone know why don't just treat this as an error? To me it seems like bad behavior on MySQL's part. Maybe it's time to switch to PostgreSQL. :P
