# Interpretable Recommender System with Frienship Networks

Author: Zeyu Li <zyli@cs.ucla.edu> or <zeyuli@g.ucla.edu>

There's nothing in this one now.

## TODO
1. `src/build_graphs.py` has been changed a lot. Need to filter out unused functions.
2. tune hyperparameters from `src/build_graphs.py`: `rwr_order` and `rwr_constant`.

## Notes
1. `src/build_graphes.py.old` is a file backuped on Nov.24.

## Preprocessing

### Yelp dataset

#### 1. Run preprocessing on yelp dataset
```bash
$ python src/prep_yelp.py preprocess
```

#### 2. Cluster the data records by cities
```bash
$ python src/prep_yelp.py city_cluster --business_min_count [bmc] --user_min_count [umc]
```

For example, if both minimum business count and minimum user count are 10, then we have:
```bash
$ python src/prep_yelp.py city_cluster --business_min_count 10 --user_min_count 10
```

Running this step will generate the statistics of datasets. We summarize them as the following.
```text
City    B-mc    U-mc    B-count    U-count
lv      10      10      32901      17146
tor     10      10      9360       8942
phx     10      10      10682      9440
```
`lv` stands for Las Vegas, `tor` stands for Toronto, and `phx` stands for Pheonix.

#### 3. Generate train, test, and validation dataset
```bash
$ python src/prep_yelp.py gen_data --ttv_ratio=10:1:1
```

#### 4. Find the results
In `./data/parse/yelp`, you would be able to see three folders:
* `preprocess`: undivided features of preprocessing.
* `citycluster`: all information clustered by cities (`lv`, `tor`, or `phx`)
* `interactions`: user-business interacton and synthesized negative samples divided into `training`,
    `testing`, and `validation`.


Among them, `citycluster` and `interactions` will be used in the future procedures.


## Building Structural Graphes

We are using structural context graphs for later computations. 
Structural context graphs can be generated beforehand.
Here's an example to generate neighbor graphs and structural context graphs:
```bash
$ python src/build_graphs.py --dataset=yelp --yelp_city=lv --rwr_order=3 --rwr_constant 0.05 --use_sparse_mat=True
```
Here are two tunable hyperparameters:
* `rwr_order`: choose between 2 and 3, number > 3 will generate a much denser graph. Defult is 3.
* `rwr_constant`: rate of re-starting. Default is 0.05.


## Run `dugrilp`


