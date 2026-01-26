
event.target.value には、**ユーザーが入力フィールドに入力したテキスト 
  （文字列）**が入ります。                                              
                                                                        
  詳しく説明                                                            
                                                                        
  event          → イベントオブジェクト（onChangeで発火）               
  event.target   → イベントが発生したDOM要素（<input>要素）             
  event.target.value → その入力欄の現在の値（文字列）                   
                                                                        
  例                                                                    
                                                                        
  89行目の <input type="text" onChange={doChangeName} />                
  に「田中」と入力した場合：                                            
                                                                        
  1. キーを押すたびに onChange イベントが発火                           
  2. doChangeName 関数が呼ばれる                                        
  3. event.target.value には "田中" という文字列が入る                  
  4. setName("田中") で state が更新される                              
                                                                        
  注意点                                                                
                                                                        
  97行目の Age フィールドも type="text"                                 
  になっているため、event.target.value は 数値ではなく文字列            
  として取得されます。                                                  
                                                                        
  // 現在のコード（文字列として取得）                                   
  setAge(event.target.value);  // "25" ← 文字列                         
                                                                        
  // 数値として扱いたい場合                                             
  setAge(Number(event.target.value));  // 25 ← 数値                     
  // または                                                             
  setAge(parseInt(event.target.value, 10));                             
                                                                        
  age の初期値が useState(0) で数値なので、型の整合性を保つなら Number()
   で変換するのが良いでしょう。