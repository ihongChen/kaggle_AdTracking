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
        - 
    - [numpy magic](https://www.kaggle.com/c/talkingdata-adtracking-fraud-detection/discussion/56105)
    - 
* eda 
    - [Feature engineering idea generator](https://www.kaggle.com/yuliagm/feature-engineering-idea-generator-numeric?scriptVersionId=3427097)
    - 