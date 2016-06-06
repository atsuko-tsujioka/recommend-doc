career-salesforce\CustomObject1Trigger\src\triggers\CustomObject1Trigger.trigger

Before Trigger:

```
if(test.isRunningTest() || (orgSettings != null && orgSettings.runDiggingUp__c)){
                    //更新データが掘り起しの場合の処理
                    system.debug(Logginglevel.WARN,'掘り起し_before');
                    Utility.debugMsg += 'beforeトリガ1回目：掘り起し判定 ' + datetime.now().addHours(9) + '\n';//デバッグ用（削除対象）
                    jobSekerTriggerHundler.diggingUpBefore();  // 2014-02-21 ★★★
                }
                
```




diggingUpBefore: called on Before Trigger

```
 public void diggingUpBefore(){

 ・・・
  //掘り起しフラグが掘り起しの場合の処理
     if(targetJobSeeker.diggingUpFlg__c == '掘り起し'){ 
    ・・・・
        //60日以上の掘り起こしの処理
        if(this.checkActivate(targetJobSeeker.njb_rjb_cjb_flag__c,oldKyusyokusya,targetJobSeeker)){
        　　　　if (!(targetJobSeeker.Field406__c != oldKyusyokusya.Field406__c
                    && targetJobSeeker.Field406__c == '送客'
                    && profileName == 'コネクトPF')) {
                    //61日以上経過している場合の条件でリフレッシュを行う
                    refreshJobSeeker(targetJobSeeker, this.jobSeekerOldMap.get(targetJobSeeker.Id), 'diggingUpAbove60' + njbJudgeStr);
                    //add_start_2016/03/24 ogino #10632
                    targetJobSeeker.Field406__c = '';
                    targetJobSeeker.FC_status__c = '';
                    //add_end_2016/03/02 ogino #10632
              }
              if(this.web == 'web'){
              　　　　refreshJobSeeker(targetJobSeeker, this.jobSeekerOldMap.get(targetJobSeeker.Id), 'diggingUpAbove60' + njbJudgeStr);


      ・・・・NJBの非常勤とかの条件に合わせて求職者関連集計関連の実装

        }else{
            //60日以内の掘り起こしの処理
            if(this.web == 'web'){
                // ★希望職種（Field16__c）からNJB、CJBを判定
                refreshJobSeeker(targetJobSeeker, this.jobSeekerOldMap.get(targetJobSeeker.Id), 'diggingUpLessThan60' + njbJudgeStr);
                ・・・・・
            }
        }
    }else if(targetJobSeeker.diggingUpFlg__c == '重複更新'){ 
       ・・・・ユーザ変更してる。リフレッシュなし


```


createHistoryRecord: called on After Trigger

```
parentJobSeeker = refreshJobSeeker(parentJobSeeker, duplicateJobSeeker, 'duplicateAbove60' + njbJudgeStr);

#リフレッシュとは関係なし
 if(njbJudgeStr == 'NJB'){
 parentJobSeeker.Field17__c == '非常勤(週20時間以上勤務)' OR 
 this.web == 'web' && parentJobSeeker.Field17__c == '非常勤' の時だけ特別処理走る。それ以外は共通


```