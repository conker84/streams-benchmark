# Results

## Andrea's test results (MacBook PRO 32 GB 8 CPU)

N.b each test has been performed 3 times, each time by starting and stopping the Docker Compose file.

### Test with the Neo4j Streams Plugin Sink

Following the test in diffirent scenarios.

#### Autocommit `true`

Last official release (v 3.5.11):

1. `5263.1578947368425`
2. `5235.602094240838`
3. `5181.347150259067`

AVG is `5226.70237975`

Beta release (v 3.5.11-beta):

1. `7352.941176470588`
2. `7407.407407407408`
3. `7352.941176470588`

AVG is `7371.09658678`

#### Autocommit `false`

Last official release (v 3.5.11):

1. `5025.125628140703`
2. `4975.124378109453`
3. `5025.125628140703`

AVG is `5008.4585448`

Beta release (v 3.5.11-beta):

1. `7462.686567164179`
2. `7751.937984496124`
3. `7633.587786259542`

AVG is `7616.07077931`

Beta release (v 3.5.11-beta) with async commit:

1. `7812.514693566321`
2. `7633.587786259542`
3. `7352.941176470588`

AVG is `7599.68121877`

### Test with a simple kafka consumer

#### Autocommit `true`

1. `6993.006993006993`
2. `7042.2535211267605`
3. `7194.244604316546`

AVG is `7076.50170615`

#### Autocommit `false`

1. `6896.551724137931`
2. `7299.270072992701`
3. `6944.444444444444`

AVG is `7046.75541386`
