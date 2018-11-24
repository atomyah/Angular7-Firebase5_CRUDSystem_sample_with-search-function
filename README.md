# AngularfirebaseStudentApp改造版

元ネタ：https://www.positronx.io/angular-7-firebase-5-crud-operations-with-reactive-forms/


## 元ネタに検索機能をつけてみました。

デモ → [Student App検索機能つき](https://student-app-2fa7d.firebaseapp.com "Student App検索機能つき")



### 変更箇所

`students-list.components.ts`に以下を追加。

```
// 検索メソッド。こんなやり方で合ってるのかわからん。
searchStudent(keyword:string) {
    let s = this.crudApi.GetStudentsList();
    s.snapshotChanges().subscribe(data => { // Using snapshotChanges() method to retrieve list of data along with metadata($key)
      this.Student = [];
      data.forEach(item => {
        let a = item.payload.toJSON();
        let dataStr = JSON.stringify(a);
        if (dataStr.search(keyword) >= 0) {
          a['$key'] = item.key;
          this.Student.push(a as Student);
        }
      })
    }) 
  }
```


`students-list.components.html`に以下を追加。

```
<div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
  <!--StudentsとListが改行されてしまうため、divで固めた-->
  <div class="col-md-4">
    <span class="h2">Students List</span>
  </div>
  <!-- It won't show if there is no student data（ *ngIf="hideWhenNoStudent" にて） -->
  <!--Inputテキスト欄とボタンを改行させないように style="display:inline-flex"必要-->
  <div class="col-md-4" style="display:inline-flex" *ngIf="hideWhenNoStudent">
    <input #keyword type="text" class="form-control" />&nbsp;
    <span class="input-group-btn">
      <button type="submit" class="btn btn-success" (click)="searchStudent(keyword.value)">検索</button>
    </span>&nbsp;
    <span class="input-group-btn">
      <button type="button" class="btn btn-secondary gap-right" (click)="ngOnInit()">検索クリア</button>
    </span>
  </div>
</div>
```


