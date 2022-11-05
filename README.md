# Partition Table in STARS: Concept and evaluations
## Instructions: 
Note: tools needed for testing include: apache2, postgresql 14, and apache jmeter
## Intro
1. please extract the project file, then extract it to the apache 2 folder (/var/www/). the extract results in the form of a folder named into.
2. create database in postgres namely "coba".
3. config "into/application/config/database.php" file for database connection
4. install pg_stat_statements in postgresql and useing default configuration. (https://www.postgresql.org/docs/current/pgstatstatements.html)
5. enable AUTOVACCUM in /etc/postgresql/14/main/postgresql.conf file. (https://www.percona.com/blog/2018/08/10/tuning-autovacuum-in-postgresql-...)
6. set "shared_buffers = 512MB" inside /etc/postgresql/14/main/postgresql.conf file
7. runing query "CREATE EXTENSION pg_stat_statements;" inside the database coba.
8. runing query "alter database coba set track_io_timing = 1;" inside the database coba.
9. restart postgresl server
10. using browser to call link "localhost/into/welcome/createDataSet/". this step can help us to setup table inside the database.
11. run apache jmeter, load and run "into/analisis/Thread Group Setup.jmx" file
12. runing query "SELECT pg_stat_statements_reset();" inside the database coba to clear other statistic data
13. run apache jmeter, load and run "into/analisis/jemeter insert non.jmx" file
14. run apache jmeter, load and run "into/analisis/jemeter insert.jmx" file
15. run apache jmeter, load and run "into/analisis/jemeter update non.jmx" file
16. run apache jmeter, load and run "into/analisis/jemeter update.jmx" file
17. run apache jmeter, load and run "into/analisis/jemeter delete non.jmx" file
18. run apache jmeter, load and run "into/analisis/jemeter delete.jmx" file
19. run apache jmeter, load and run "into/analisis/jemeter select non.jmx" file
20. run apache jmeter, load and run "into/analisis/jemeter select.jmx" file
21. finally, runing query "

SELECT query, calls, round(total_exec_time::numeric, 5) AS total_time, rows, 

100.0 * shared_blks_hit / nullif(shared_blks_hit + shared_blks_read, 0) AS hit_percent,

round(mean_exec_time::numeric, 5) AS mean,

round(stddev_exec_time::numeric, 5) AS stddev,

round(min_exec_time::numeric, 5) AS min,

round(max_exec_time::numeric, 5) AS max,

round((100 * total_exec_time / sum(total_exec_time::numeric) OVER ())::numeric, 5) AS percentage_cpu

FROM pg_stat_statements

where query ilike '%participant%' and (query ilike '%select%' or query ilike '%insert%' or query ilike '%update%' or query ilike '%delete%')

order by query desc; " 
22. inside the database coba to clear other statistic data. this process can show result for sql sintaxs "insert, uodate, delete, and select" for table partition and table no partition
