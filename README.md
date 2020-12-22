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
## bpr model for bundle
training_data = all_data[:int(0.8*data_size)]
test_data =[int(0.8*data_size):]

## bpr_item model for items
training_data_2 =all_item_data[:int(0.8*len(all_item_data))]
test_data_2 =all_item_data[int(0.8*len(all_item_data)):]
```

## graph sampling (图采样）
```
def check_tuple(tuple_1, tuple_2, user_bundle_map)
(whether the tuple 1 and tuple_2 are connected
    True： unconnected
    False: connected)

def graph_sampling(n_samples, training_data, user_bundle_map)
(sgd_users=[]
 sgd_pos_items, sgd_neg_items=[], []
 while n_samples>0
    choose Tuple_1 and Tuple_2 randomly (user, bundle)
    iteration =100
    while tuple_1 and tuple_2 are connected
        choose tuple_2 randomly and reduce iteration by one
        if iteration=0; break
    if iteration=0; continue; (which means tuple1 and 2 connected)
    sgd_users.append (tuple_1[0])
    sgd_neg_items.append(tuple_2[1])
    sgd_pos_items.append(tuple_1[1])
    
    sgd_users.append(tuple_2[0])
    sgd_neg_items.append(tuple_1[1])
    sgd_pos_items.append(tuple_2[1])
    n_samples-=2
   return sgd_users, sgd_pos_items, sgd_neg_items)
```

## Generate training data through graph sampling
```
sgd_train_users_items, sgd_train_pos_items, sgdz-train_neg_items = graph_sampling (len(training_data_2)*30, training_data_2, user_item_map)
```

## Generate test data items
```
def data_to_dict(data)
    data_dict = defaultdict(list)
    items = set()
    for (user, item) in data:
        data_dict[user].append(item)
        items.add(item)
    return data_dict, set(data_dict.keys()), items
    
def get_test_data_items (test_data, train_data）
(   users, pos_items, neg_items = [], [], []
    train_dict, train_users, train_items = data_to_dict(train_data)
    test_dict, test_users, test_items = data_to_dict(test_data)
    for i, user in enumerate(test_dict.keys())
        if user in train_users:
            for pos_item in test_dict[user]:
                if pos_item in train_items:
                    for neg_item in train_items:
                        if neg_item not in test_dict[user] and neg_item not
                        in train_dict[user]
                            users.append(user)
                            pos_items.append(pos_item)
                            neg_items.append(neg_item)
  return users, pos_items, neg_items
  )
  The purpose of this function is to choose the appropriate positive and negative items to test the correctness of the recommendation
```
