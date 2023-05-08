Download Link: https://assignmentchef.com/product/solved-tdt4225-exercise-2
<br>
This  assignment  will  look  at  an  open  dataset  of  trajectories,  and  your  task  is  to  create   tables,  clean  and  insert  data,  and  to  make  queries  to  answer  some  questions.  Your   Python  program  will  handle  this  functionality.  The  program  is  inspired  by  the  social   media  workout  application <a href="https://www.strava.com/features">Strava</a> ,   where  users  can  track  activities  like  running,   walking,  biking  etc  and  post  them  online  with  stats  about  their  workout.

Along  with  this  assignment  sheet,  you  have  been  given  several  files:

<ul>

 <li>Dataset –  Modified  dataset  based  on  Geolife  GPS  Trajectory  dataset  (please  use   this  dataset,  instead  of  the  one  from  the    The  dataset  from  the  website   contains  some  files  that  may  give  you  an  error).</li>

 <li>py –   a  Python  class  that  connects  to  MySQL  on  the  virtual   machine.</li>

 <li>py –  a  Python  class  with  examples  on  how  to  create  tables,  insert,   fetch  and  delete  data  in  MySQL.</li>

 <li>txt –   a  file  containing  some  pip  packages  that  should  be  used   in  this  assignment.</li>

 <li>txt –   a  file  containing  the  ID  of  all  users  who  have  labeled  their   data.  This  is  found  in  the  Dataset.zip  file.</li>

 <li>User-Guide-1.3.pdf –   an  official  user  guide  to  the  Geolife    This  is   found  in  the  Dataset.zip  file.</li>

</ul>




<strong>It  is <u>strongly   </u>  recommended  that  you  read  through  and  understand  the  whole   assignment  sheet  before  you  start  solving  the  tasks.  There  are  also  some  tips  at  the   bottom  of  this  assignment,  which  could  be  very  handy!   </strong>

<strong>  </strong>

1

<strong>  </strong>

<h1>Dataset  –  Geolife  GPS  Trajectory  dataset</h1>

<a href="https://www.microsoft.com/en-us/research/publication/geolife-gps-trajectory-dataset-user-guide/">Geolife  GPS  Trajectory  dataset</a>  is  an  open  source  dataset  collected  by  Microsoft  from   2007-2011.  This  dataset  recorded  a  broad  range  of  users’  outdoor  movements,   including  not  only  life  routines  like  going  home  and  going  to  work  but  also  some   entertainments  and  sports  activities,  such  as  shopping,  sightseeing,  dining,  hiking,  and   cycling.  The  user  data  is  mainly  from  Beijing,  China,  but  also  from  the  US  and  Europe.

The  dataset  is  provided  in  the tdt4225-assignment2.zip             and   has  some   differences  from  the  one  online,  for  the  purpose  of  this  assignment.  It  contains  182   users  that  have  tracked  18  669  activities  in  total.  Each  activity  contains  a  number  of   GPS-points,  which  we  call  TrackPoints.  In  total  there  are  over  24  million  trackpoints  in   this  dataset.




In  the tdt4225-assignment2.zip       you   will  find  the  official  user  guide  for  the   dataset.  Be  sure  to  read  through  this  before  you  start  the  task.  Here  is  also  a  short   summary  of  the  dataset, <u>understanding             this  is  crucial</u>  for  completing  the  assignment:

The  folder  named  Data  contains  all  the  182  users,  labeled  from  “000”  to  “181”.  Each   user  has  a  Trajectory-folder  where  each  activity  is  stored  as  a  .plt  file.  Each  .plt  file   contains  several  trackpoints.




Line  1…6  in  the  .plt  files  are  useless  in  this  dataset,  and  can  be  ignored.  Trackpoints  are   described  in  following  lines,  one  for  each  line:




Field  1:  Latitude  in  decimal  degrees.

Field  2:  Longitude  in  decimal  degrees.

Field  3:  All  set  to  0  for  this  dataset,  i.e.  you  don’t  need  this  field.

Field  4:  Altitude  in  feet  (-777  if  not  valid).

Field  5:  Date  –  number  of  days  (with  fractional  part)  that  have  passed  since

12/30/1899.

Field  6:  Date  as  a  string.

Field  7:  Time  as  a  string.

Note  that  field  5  and  field  6&amp;7  represent  the  same  date/time  in  this  dataset.  You  may   use  either  of  them.




Example:

39.906631,  116.385564,  0,  492,  40097.5864583333,  2009-10-11,  14:04:30

39.906554,  116.385625,  0,  492,  40097.5865162037,  2009-10-11,  14:04:35

2

Some  users  have  also  labeled  their  data  with  transportation  modes.  Possible   transportation  modes  are: <em>walk,</em>        <em>  bike,  bus,  taxi,   car,  subway,  train,  airplane,  boat,  run  and   motorcycle </em>.  We  have  provided  you  with  the  ids  of  the  users  that  have  labeled  their  data   in labeled_ids.txt      ,  which  will  be  used  for  the  tasks.  The  labels  contain  start  time   and  end  time  (both  date  and  time)  along  with  the  transportation  mode  for  each  activity.

<table width="550">

 <tbody>

  <tr>

   <td width="192"> Example:</td>

   <td width="192"> </td>

   <td width="166"> </td>

  </tr>

  <tr>

   <td width="192"><strong>Start  Time   </strong></td>

   <td width="192"><strong>End  Time   </strong></td>

   <td width="166"><strong>Transportation  Mode   </strong></td>

  </tr>

  <tr>

   <td width="192">2008/04/02  11:24:21</td>

   <td width="192">2008/04/02  11:50:45</td>

   <td width="166">bus</td>

  </tr>

  <tr>

   <td width="192">2008/04/03  01:07:03</td>

   <td width="192">2008/04/03  11:31:55</td>

   <td width="166">train</td>

  </tr>

 </tbody>

</table>

<strong>  </strong>

<h1>Tables</h1>

<strong>  </strong>

<table width="600">

 <tbody>

  <tr>

   <td width="159"><strong>User   </strong><u>id  –  string</u>   has_labels  –  boolean <strong>  </strong></td>

   <td width="224"><strong>Activity   </strong><u>id  –  int</u>user_id  –  string  (Foreign  Key)   transportation_mode  –  string     start_date_time  –  datetime   end_date_time  –  datetime</td>

   <td width="218"><strong>TrackPoint     </strong><u>id   –  int</u>activity_id  –  int  (Foreign  Key)   lat  –  double   lon  –  double   altitude  –  int   date_days  –  double   date_time  –  datetime <strong>  </strong></td>

  </tr>

 </tbody>

</table>




Datetime  should  be  consistent  throughout  your  tables,  e.g.  in  the  format

YYYY-MM-DD  HH:MM:SS.




<strong>  </strong>




<h1>Setup  database</h1>

These  steps  will  guide  you  through  how  to  connect  to  the  virtual  machine  from  your   computer  and  create  a  MySQL-database  user  with  privileges.

<ol>

 <li>First, you  need  to  use  NTNU’s  VPN  to  access  the  virtual    Log  onto   NTNU-VPN  with  your  Feide  user  (check  out <a href="https://innsida.ntnu.no/wiki/-/wiki/English/Install+vpn">this</a> <u>      </u><a href="https://innsida.ntnu.no/wiki/-/wiki/English/Install+vpn">  link</a>  if  you  have  not  done  this   before).</li>

</ol>




<ol start="2">

 <li>Open your  terminal  and  enter  the  following  command:</li>

</ol>

ssh  <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="94edfbe1e6cbe1e7f1e6faf5f9f1d4e0f0e0a0a6a6a1b9ececbafdf0fdbafae0fae1bafafb">[email protected]</a>  where

<strong>“your_username”</strong>  is  the  Feide-username  and <strong>“xx”</strong>             is  your  group  number  for  this   project  (01,  02,  03…  etc).

(If  this  message  pops  up,  enter <strong>yes </strong>           :

The  authenticity  of  host  ‘tdt4225-xx.idi.ntnu.no  (xxx.xxx.xxx.xxx)’  can’t  be  established.

ECDSA  key  fingerprint  is  …

Are  you  sure  you  want  to  continue  connecting  (yes/no)?  yes)

You  will  then  be  asked  to  enter  your  password,  use  the  Feide-password  here.

If  the  password  is  correct,  you  have  now  successfully  logged  in!

<ol start="3">

 <li>Now we  want  to  create  a  MySQL-user  that  the  group  will  use  during  this     First,  enter sudo   mysql   in  your  terminal  windows  (which  is  now   inside  the  virtual  machine)  and  type  in  your  Feide  password.  If  successful,  you   are  now  on  the  MySQL  server  and  can  write  MySQL-queries  directly  in  this   window: mysql&gt;   “some  query”.</li>

</ol>




<ol start="4">

 <li>Run the  following  commands  in  MySQL  to  create  a  new  user  and  give  the  user   admin</li>

</ol>

mysql&gt;  CREATE  USER  ‘YOUR_USERNAME_HERE’@’%’  IDENTIFIED  BY

‘YOUR_PASSWORD_IN_PLAIN_TEXT_HERE’;

mysql&gt;  GRANT  ALL  PRIVILEGES  ON  *.*  TO  ‘YOUR_USERNAME_HERE’@’%’WITH  GRANT  OPTION;




Yes,  write  the  username  using  quotas  ‘’.  Here  is  an  example  for   username=testuser  and  password=test123:

CREATE  USER  ‘testuser’@’%’  IDENTIFIED  BY  ‘test123’;

GRANT  ALL  PRIVILEGES  ON  *.*  TO  ‘testuser’@’%’  WITH  GRANT

OPTION;




Now  your  new  user  has  been  granted  admin  rights.  The  %  sign  signifies  a   wildcard,  meaning  that  your  new  user  will  have  access  to  all  schemas  in  MySQL.   You  will  want  to  flush  the  privileges  so  that  MySQL  will  load  your  user  with  its

new  rights.  Type mysql&gt;   FLUSH  PRIVILEGES;




To  check  if  the  user  is  created,  type mysql&gt;       SELECT  User  FROM  mysql.user;.   If  you  see  your  newly  created  user  in  the  list,  you  have  created   the  user  successfully.

<ol start="5">

 <li>Let’s create a  database.  Type mysql&gt;         SHOW  DATABASES;,   there  should  be   four  default  databases,  ignore  these.</li>

</ol>

Create  a  new  database  by  typing mysql&gt;             CREATE  DATABASE  db_name;,    where <strong>“db_name”</strong>       is  your  chosen  name  of  the  database,  example mysql&gt;

CREATE  DATABASE  test_db;.   Try  typing  mysql&gt;   SHOW  DATABASES;  again  and  check  if  the  database  is  there.  If  it  is  there,  congratulations!




To  exit  the  MySQL  program,  simply  type exit    and  you  are  back  at  the  virtual  machine.

Now  you  have  to  type sudo            service  mysql  restart  in   the  terminal.  Once  this  is   complete,  you  should  be  able  to  use  the  database  from  your  Python  program  (or  the   MySQL  terminal).




<h1>Setup  Python</h1>

We  will  be  using  a <a href="https://github.com/mysql/mysql-connector-python">MySQL</a> <u>  </u><a href="https://github.com/mysql/mysql-connector-python">  connector  for  Python</a>  in  this  assignment.  If  you  for  some   reason  would  like  to  complete  the  assignment  in  another  language,  you  are  allowed  to   do  so. <strong><em>(</em></strong> <strong><em>NB!  We  will <u>not </u>  provide  support  or  documentation  for  other  languages,  so  we   strongly  encourage  you  to  use  Python.)   </em></strong>

<strong><em>  </em></strong>

With  the  files  provided  in  this  assignment,  you  will  find  a requirements.txt,

DbConnector.py  and  example.py   ,  among  others.




<ol>

 <li>To set  up  the  required  pip-packages  for  this  assignment,  simply  run     pip  install  -r  txt  while   you  are  in  the  directory  with   the  requirements-file.  This  will  install  the  connector,  along  with <a href="https://pypi.org/project/tabulate/">tabulate</a>   (used   to  print  pretty  tables  in  python)  and <a href="https://pypi.org/project/haversine/">haversine</a>   (used  to  calculate  distance   between  two  coordinates).</li>

 <li>Now, open py        and   look  at  the  code.  There  will  be  a  TODO  there   to  set  up  the  database  correctly  with  the  settings  from  the  database  setup.   Update  the  values  accordingly:</li>

</ol>

<strong>Host </strong>=   tdt4225-xx.idi.ntnu.no,  where  xx  is  your  group  number

<strong>Database </strong>=   the  name  you  provided  for  your  database  in  step  5  of  the  database   setup  (test_db  from  the  example)

<strong>User </strong>=   the  username  provided  in  step  4,  (testuser  from  the  example)

<strong>Password </strong>=   the  password  provided  in  step  4  (test123  from  the  example)

<strong>Port </strong>=   this  is  optional,  and  default  is  3306

<ul>

 <li>Passwords should  be  stored  as  an  environmental  variable  if  your  code  is   public.  See <a href="https://able.bio/rhett/how-to-set-and-get-environment-variables-in-python--274rgt5">here</a> <u>    </u>,   or  search  Google  to  find  out  how  to  do  it.</li>

</ul>

<ol start="3">

 <li>Open py            ,  the  file  has  a  main  method  that  creates  a  connection  to  the   database  from  DbConnector,  creates  a  table  named  “Person”,  inserts  some   names  in  the  database,  displays  the  data,  and  then  drops  the  table.   Try  to  run  the  code.  If  everything  is  set  up  correctly,  you  will  establish  a   connection  to  the  database  and  insert/delete  data.  You  can  use  this  file  for   inspiration  when  solving  the  tasks  in  this  assignment.  <strong> </strong> <strong> </strong></li>

</ol>

<strong>  </strong>

<h1>Tasks</h1>

The  tasks  are  divided  into  three  parts:  Part  1  will  focus  on  cleaning  and  inserting  the   data  into  defined  tables.  Part  2  will  focus  on  writing  queries  to  the  database  to  gain   knowledge  of  the  dataset.  Part  3  will  focus  on  writing  a  report  where  you  discuss  your   answers.

We  recommend  looking  at  the <a href="https://dev.mysql.com/doc/connector-python/en/connector-python-examples.html">documentation</a> <u>    </u><a href="https://dev.mysql.com/doc/connector-python/en/connector-python-examples.html"> </a> for  the  MySQL-Python  connector   before  you  start  coding.

<strong>  </strong>

<h2>Part  1</h2>

In  this  task  you’ll  clean  and  insert  the  Geolife  dataset  into  your  own  MySQL  database,   to   be  able  to  solve  the  questions  in  Part  2.  In  the  section  “Geolife  GPS  Trajectory   dataset“  above,  you’ll  find  all  the  information  you  need  about  the  dataset  you  are   handed,  and  in  the  section  “Tables”  you’ll  find <u>our             suggestion</u>  for  how  to  structure  the   dataset  in  your  MySQL  database.   You  are  free  to  do  it  in  another  way  as  well,  but   please <u>discuss         in  your  report</u>  why  you  do  things  differently.




Write  a  Python  program  that  does  the  following:

<ol>

 <li>Connects to  the  MySQL  server  on  your  Ubuntu  virtual</li>

 <li>Creates and  defines  the  tables  User,  Activity  and  TrackPoint</li>

 <li>Inserts the  data  from  the  Geolife  dataset  into  your  MySQL  database</li>

</ol>

○    Here,  we  require  a  bit  of  data  cleaning  and  integration.  Study  the  dataset   and  the  tables <u>closely </u>  to  understand  which  data  goes  where.

○  Insert  data  into  User,  Activity  and  TrackPoint,  in  that  order.

■    E.g.  you  cannot  create  an  Activity  (with  the  filed  user_id)  without   having  a  User  with  that  id.

○    Iterating  through  the  dataset  in  the  directories  can  be  done  using <a href="https://www.geeksforgeeks.org/os-walk-python/">os.walk</a> <u>   </u>   method,  but  you  are  free  to  use  other  methods  if  you  find  them  better  for   your  solution.

○    When  matching  transportation_mode  from  the labels.txt   files   to  the   activities,  we  only  consider  exact  matches  on  starttime   and  end  time,  i.e.   if  you  find  a  match  in  start  time,  but  not  in  end  time,  or  vice  versa,  it   should  not  be  included.  Additionally,  there  are  some  labeled  activities  in   labels.txt  that   will  not  have  a  match  amongst  the  activities.

<strong>○ Important! </strong>Sometimes   it  is  wise  to  limit  the  dataset  we’re  working  with.   24  million  TrackPoints  is  a  lot.  Therefore  we  only  want  you  to  insert   activities  that  have <strong>fewer</strong> <strong>  than  or  exactly  2500  trackpoints</strong>  in  them,  i.e.   when  inserting  activities  into  the  database,  check  that  the  size  of  the   .plt-files  do  not  exceed  2500  lines  (excluding  the  headers,  of  course).   <strong>  </strong>When  you  insert  TrackPoints,  the  same  rule  applies  (you  cannot  link  a   trackpoint  to  an  activity  that  is  dropped,  anyway).

○    Inserting  the  trackpoints  may  take  a  while  (could  potentially  be  10-15   mins)!  Instead  of  inserting  one  row  of  trackpoints  at  a  time,  find  out  if   there  is  a  way  to  insert  batches  of  data  instead.




<h2>Part  2</h2>

<u>Some  of  the  tasks  can  be  answered  using  MySQL-queries  only,  while  some  might</u>   <u>require  both  queries  and  Python  code  to  manipulate  the  data  correctly.</u>

Answer  the  following  questions  by  writing  a  Python  program  using  MySQL-queries

(like  in example.py          ):

<ol>

 <li>How many  users,  activities  and  trackpoints  are  there  in  the  dataset  (after  it  is   inserted  into  the  database).</li>

 <li>Find the  average,  minimum  and  maximum  number  of  activities  per</li>

 <li>Find the  top  10  users  with  the  highest  number  of</li>

 <li>Find the  number  of  users  that  have  started  the  activity  in  one  day  and  ended  the  activity  the  next</li>

 <li>Find activities  that  are  registered  multiple    You  should  find  the  query  even  if  you  get  zero  results.</li>

 <li>Find the  number  of  users  which  have  been  close  to  each  other  in  time  and  space  (Covid-19  tracking).  Close  is  defined  as  the  same  minute  (60  seconds)  and  space  (100  meters).</li>

 <li>Find all  users  that  have  never  taken  a</li>

 <li>Find all  types  of  transportation  modes  and  count  how  many  distinct  users  that   have  used  the  different  transportation    Do  not  count  the  rows  where  the   transportation  mode  is  null .</li>

</ol>




<ol start="9">

 <li>a) Find  the  year  and  month  with  the  most</li>

 <li>b) Which user  had  the  most  activities  this  year  and  month,  and  how  many   recorded  hours  do  they  have?  Do  they  have  more  hours  recorded  than  the  user   with  the  second  most  activities?</li>

</ol>

10.Find  the  total  distance  (in  km) <u>walked </u>  in  2008,  by  user  with  id=112.

11.Find  the  top  20  users  who  have  gained  the  most  altitude <u>meters         </u>.

○ Output  should  be  a  table  with  (id,  total  meters  gained  per  user).

○    Remember  that  some  altitude-values  are  invalid




○     Tip:   ∑(<em>tp</em> <em><sub>n</sub></em>.<em>altitude </em>− <em>tp</em> <em><sub>n</sub></em><sub>−1</sub>.<em>altitude</em>),  <em>tp</em> <em><sub>n</sub></em>.<em>altitude</em> &gt; <em>tp</em> <em><sub>n</sub></em><sub>−1</sub>.<em>altitude</em>




12.Find  all  users  who  have  invalid  activities,  and  the  number  of  invalid  activities  per   user

○    An  invalid  activity  is  defined  as  an  activity  with  consecutive  trackpoints   where  the  timestamps  deviate  with  at  least  5  minutes.

<strong>  </strong>

<h2>Part  3</h2>

Write  a  short  report  (see report-template.docx   in   the

tdt4225-assignment2.zip )  where  you  will  display  and  discuss  your  results  from   the  tasks.  Include  screenshots  from  both  part  1  (showing  top  10  rows  from  all  of  your   tables  is  sufficient)  and  part  2  (all  the  results  to  each  task).




Hand  in  both  the  report  (as  PDF)  and  your  code  to  BlackBoard  within <u>Oct   9,  at  16:00 <em>.</em></u> <em> </em>

<strong>  </strong>

<h1>Tips</h1>

<ul>

 <li>Using the  MySQL  terminal  on  your  Ubuntu  machine  could  be  useful  for   checking  simple  queries  without  having  to  use  Python  and  the</li>

</ul>

○  E.g.  just  checking  if  the  data  is  correct  “SELECT  *  FROM  User  LIMIT  5”

○    Open  the  MySQL  terminal  by  typing sudo             mysql  ,   log  in  with   Feide-password  and  type  use   name_of_db  to   do  this.

<ul>

 <li>Using dictionaries/hashmaps  are  faster  for  lookups  than  lists/arrays  in  your   Python  code</li>

 <li>When extracting  transportation_mode  from  the txt    files,  remember   that  you  only  have  to  consider  the  user-ids  found  in  the labeled_ids.txt        ,  as   they  are  the  only  users  where  transportation_mode  may <u>not           </u>  be  null.</li>

 <li>Start_time and  end_time  for  activities  can  be  found  by  looking  at  the  date  and   time  for  the  first  and  last  trackpoint  in  each  .plt-file</li>

 <li>Remember to  keep  track  of  activity-ids  when  inserting  trackpoints,  so  that  each   trackpoint  has  the  correct  foreign  key  to  the  Activity  table</li>

 <li>Remember foreign  key  cascade  rules</li>

</ul>

8

<ul>

 <li>How to  use  and  take  advantage  of  the  datetime  data  type  in  your  queries  can  be   found <a href="https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html">here</a> <u>            </u>.</li>

 <li>Using <a href="https://www.mysqltutorial.org/mysql-variables/">variables</a> <u> </u><a href="https://www.mysqltutorial.org/mysql-variables/">  in  MySQL</a>  may  come  in  handy</li>

 <li>As stated,  using <a href="https://pypi.org/project/haversine/">Haversine</a> <u>   </u><a href="https://pypi.org/project/haversine/"> </a> for  calculating  distance  is  recommended</li>

 <li>As stated,  using <a href="https://pypi.org/project/tabulate/">Tabulate</a> <u>     </u><a href="https://pypi.org/project/tabulate/"> </a> for  printing  tables  in  your  Python  program  is   recommended</li>

 <li>Optional –  if  you  want  to  visualize  the  data  in  the  dataset,  you  can  get  inspired   <a href="https://heremaps.github.io/pptk/tutorials/viewer/geolife.html">here</a>.</li>

</ul>


