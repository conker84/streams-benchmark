# Results

## Andrea's test results (MacBook PRO 32 GB 8 CPU)

N.b each test has been performed 3 times, each time by starting and stopping the Docker Compose file.

### Test with the Neo4j Streams Plugin Sink

Following the test in diffirent scenarios.

#### Autocommit `true`

Last official release (v 4.0.5):

1. `8771.929824561403`
2. `8695.652173913044`
3. `8849.557522123894`

AVG is `8772.3798402`

Beta release (v 4.0.5-beta):

1. `16949.15254237288`
2. `16129.032258064517`
3. `17857.14285714286`

AVG is `16978.4425525`

#### Autocommit `false`

Last official release (v 4.0.5):

1. `9090.90909090909`
2. `8849.557522123894`
3. `9259.25925925926`

AVG is `9066.57529076`

Beta release (v 4.0.5-beta):

1. `14925.373134328358`
2. `16666.666666666668`
3. `17857.14285714286`

AVG is `16483.060886`

Beta release (v 4.0.5-beta) with async commit:

1. `16949.15254237288`
2. `17857.14285714286`
3. `18518.51851851852`

AVG is `17774.9379727`

### Test with Kafka Connect Sink

#### Parallelization `true`

Last official release (v 1.0.9):

1. `13513.513513513513`
2. `14492.753623188406`
3. `14084.507042253521`

AVG is `14030.2580597`

Beta release (v 1.0.9-beta):

1. `14084.507042253521`
2. `14084.507042253521`
3. `14705.882352941177`

AVG is `14226.4260947`

#### Parallelization `false`

Last official release (v 1.0.9):

1. `14492.753623188406`
2. `12048.192771084337`
3. `14285.714285714286`

AVG is `13608.8868933`

Beta release (v 1.0.9-beta):

1. `14492.753623188406`
2. `14925.373134328358`
3. `14925.373134328358`

AVG is `14781.1666306`

### Test with a simple kafka consumer

#### Autocommit `true`

1. `15873.015873015873`
2. `16393.44262295082`
3. `16393.44262295082`

AVG is `16219.9670396`

#### Autocommit `false`

1. `15625.0`
2. `15151.515151515152`
3. `15873.015873015873`

AVG is `15549.8436748`
