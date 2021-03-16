- describe
  - テストの対象を記述
  - 一番外側のdescribeはファイル全体の主題

- context
  - テストの内容を状況や状態のバリエーションごとに分類
  - 技術的にはdescribeのエイリアスであるため、どちらを使ってもSpecは動作する

- before
  - 対応するdescribeやcontextの　領域のテストコードを実行する前に実行される
  - 領域全体の前提条件を表す場所だとも言える
  - itが実行されるたびに実行される

- it
  - 期待する動作を記述する
  - 想定していたとおりに動作しておらずSpecが予期せぬ例外が発生するとError
  - 例外ではないが想定と違う場合はFailure


```rb
describe '〜機能', type: system do
  
  describe '登録' do
    
    context 'Aの場合' do
      before do
      end
      
      it 'XXXする' do
      end
    end
    
    context 'Bの場合' do
      before do
      end
      
      it 'xxxする' do
      end
    end
    
    describe '削除' do
      .........
      .........
    end
    
  end
      
```
