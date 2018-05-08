Lesson learnt from this competition

* target (mean) enocoding
    - [kernel](https://www.kaggle.com/ogrellier/python-target-encoding-for-categorical-features)
    - becarefull data leakage: oof or stratifed K-Fold
    ```python
    from sklearn.model_selection import StratifiedKFold
    print('copy trn_df to make new features')
    train_new = trn_df.copy()

    cols = ['ip','app','channel','os','device']
    for col in cols :
        train_new[col+'_mean_target'] = 0
        
    y_tr = trn_df.is_attributed.values.astype(np.int8) # target 
    skf = StratifiedKFold(4, random_state=0)
    skf.get_n_splits(X=trn_df,y=y_tr)


    for fold, (tr_idx, val_idx) in enumerate(skf.split(train_new,y_tr)):
        
        ## generate features 
        
        X_tr ,X_val = trn_df.iloc[tr_idx], trn_df.iloc[val_idx]
        
        print('fold:{} mean encoding...'.format(fold),end='\n\t')
        ## print('tr_idx:{}'.format(tr_idx))
        for col in cols:        
            print(col,end='\t')
            means = X_val[col].map(X_tr.groupby(col).is_attributed.mean()) ## map mean encoding in X_tr to X_val
            X_val[col + '_mean_target'] = means.astype('float16')
            
        train_new.iloc[val_idx] = X_val
        print('')    
        del X_tr,X_val;gc.collect()
                
    prior = trn_df.is_attributed.mean()
    train_new[[col+'_mean_target' for col in cols]] = train_new[[col+'_mean_target' for col in cols]].fillna(prior)

    trn_df = train_new
    del train_new;gc.collect()
    print('complete')

    ```
    
* memory issue
    - lgbm
        - `two_round = True`
        - `scale_pos_weight` : [meaning](https://stats.stackexchange.com/questions/243207/what-is-the-proper-usage-of-scale-pos-weight-in-xgboost-for-imbalanced-datasets?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)
        >  Generally, the `Scale_pos_weight` is the ratio of number of negative class to the positive class. Suppose, the dataset has 90 observations of negative class and 10 observations of positive class, then ideal value of scale_pos_Weight should be 9. You can check the [following link](http://xgboost.readthedocs.io/en/latest/parameter.html).        


            
    - [numpy magic](https://www.kaggle.com/c/talkingdata-adtracking-fraud-detection/discussion/56105)
    - use Database instead of python/R to group by
    
* eda 
    - [Feature engineering idea generator](https://www.kaggle.com/yuliagm/feature-engineering-idea-generator-numeric?scriptVersionId=3427097)
* fe
    - [WoE/IV](https://blog.csdn.net/kevin7658/article/details/50780391)
    - [feature engineering guide](https://github.com/h2oai/h2o-meetups/blob/master/2017_11_29_Feature_Engineering/Feature%20Engineering.pdf)
* top kaggler method
    - [4th place (brief) tips](https://www.kaggle.com/c/talkingdata-adtracking-fraud-detection/discussion/56243)
    - [Solution to Duplicate Problem by Reverse Engineering (0.0005 Boost)](https://www.kaggle.com/c/talkingdata-adtracking-fraud-detection/discussion/56268)
    - [solution #6 overview](https://www.kaggle.com/c/talkingdata-adtracking-fraud-detection/discussion/56283)
    - [9th place](https://www.kaggle.com/c/talkingdata-adtracking-fraud-detection/discussion/56279)
    - [11th place features](https://www.kaggle.com/c/talkingdata-adtracking-fraud-detection/discussion/56250)
    