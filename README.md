# lib-core

> note: only works on [MLSQL Latest 2.1.0-SNAPSHOT](http://download.mlsql.tech/2.1.0-SNAPSHOT/) version


Ray 1.3.0：

```
pip install Cython
pip install ray==1.3.0
pip install aiohttp psutil setproctitle grpcio pandas
pip install watchdog requests click uuid plotly vega_datasets
pip install pyjava>=0.2.8.8
```

Conda recommended.

## dataset.vega_datasets

Make sure python lib vega_datasets installed.  
Then you can save the vega_datasets to any path with
following code:

```sql
include lib.`github.com/allwefantasy/lib-core` where 
libMirror="gitee.com" and -- if you'r in china, set proxy
alias="libCore";

-- set pythonEnv="source activate dev";
-- once vega_datasets.mlsql is inclued then you
-- you have `!dumpData` command available.
include local.`libCore.dataset.vega_datasets`;

-- dump data to object store
!dumpData /tmp/veca_datasets;

-- Check the data
load parquet.`/tmp/veca_datasets` as output;
```

### dataset.vega_datasets_visual

Visualize vega_datasets.

```sql
load parquet.`/tmp/veca_datasets` as data;

set inputTable="data";
include local.`libCore.dataset.vega_datasets_visual`;
```

### dataset.mnist

```sql

include lib.`github.com/allwefantasy/lib-core` where 
force="true" and
libMirror="gitee.com" and -- proxy configuration.
alias="libCore";

-- dump minist data to object storage
include local.`libCore.dataset.mnist`;
!dumpData /tmp/mnist;
```

### alg.mnist*


```sql

-- load the data we dumped before and named mnist_data
load parquet.`/tmp/mnist` as mnist_data;

-- configure python env and setup the input table name.
set inputTable="mnist_data";
set pythonEnv="source /Users/allwefantasy/opt/anaconda3/bin/activate ray1.3.0";

-- train the mnist data without Ray cluster.
include local.`libCore.alg.mnist_train`;
-- save the model in data lake.
save overwrite mnist_data_out as delta.`ai_model.mnist_model`;

-- train with Ray Cluster.
select "RAY_ADDRESS" as k, "127.0.0.1:10001" as v as rayConfig;
include local.`libCore.alg.mnist_train_on_ray`;
-- save the model in data lake.
save overwrite mnist_data_out as delta.`ai_model.mnist_on_ray_model`;

-- show the model we saved in data lake.
load delta.`ai_model.mnist_on_ray_model` as mnist_on_ray_model;
```

## mnist Predict Example 

[minist_predict_example.mlsql](https://github.com/allwefantasy/lib-core/blob/main/alg/minist_predict_example.mlsql)



