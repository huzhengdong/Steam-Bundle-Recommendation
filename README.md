# Steam-Bundle-Recommendation
This is a forked repository from https://github.com/technoapurva/Steam-Bundle-Recommendation

To complete the project, I will do some small changes to the project

## Input dataset
```
## bundle_recommend_generate.ipynb
input dataset:
(1) items_set: The set of items
(2) item_data: The set of item data
(3) item_id_lookuo:  The map from item to id
(4) item_name_map:   The map from item to name

(5) bundle_item_map:  The map from bundle to items
(6) bundle_diversity_map:   The map from bundle to diversity

(7) user_bundle_map:  The map from user to bundles
(8) user_item_map:    The map from user to items

```

## Generate dataset
```
(1) item_data_map = dict()
    for item in item_data
    item_data_map[int(item['appid'])]=item
(2) tags_set =set()
    for item in item_data:
      for tag in item['tags']:
        tags_set.add(tag)
(3) tags_map=dict()
    for i, tag in enumerate(tags_set):
        tags_map[tag]=i;
（4） all_data = []
    for user, bundles in user_bundle_map.items():
     for bundle in bundles
     all_data.append((user, bundle))
(5)  all_item_data = []
    for user, items in user_item_map.items():
        for item in items
            all_item_data.append((user, item))
```

## preprocessing
```
random.shuffle(all_data)
data_size=len(all_data)
```

## training and test data
```
bpr model for bundle
training_data = all_data[:int(0.8*data_size)]
test_data =[int(0.8*data_size):]

bpr_item model for items
training_data_2 =all_item_data[:int(0.8*len(all_item_data))]
test_data_2 =all_item_data[int(0.8*len(all_item_data)):]
```


