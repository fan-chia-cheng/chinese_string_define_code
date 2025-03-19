### chinese_string_define_code
##Step1 
將EXCEL，同一欄中的中文字設定固定長度為20，進行同一固定長度切割後，分別做為下一個列的欄資料，可讓工作項目排列美觀。
問題點:英文字母與中文字，雙方所占空間不同，會有長短不一的問題
解決方法，分別判斷ASCII碼範圍後，再使用不同長度判斷後，最後加總合為20。

檔案名稱1-1:檔案名稱1-2:使用pandas(配合EXCEL結構、讀入EXCEL檔案)、openpyxl(後續不再使用，calc程式儲存以xlsm為優先，儲存EXCEL檔案)模組;
P_data 是原始EXCEL，需要注意資料類型，新增欄位21(U為'合併')的資料指定由前面2(工作項目及說明)的資料進行放置;
檔案名稱1-3,1-4,1-6:針對工作項目內容的開頭文字做正則(幫忙在前一欄位填入'/'，也可手動填入)，非單價為Nan的填入0,'0',''等變化。
檔案名稱1-7:資料副本Q_data(原稿P_data ，Q_data統計實做實算與總額結算項次紀錄)
檔案名稱1-8:資料副本Q_data(原稿P_data ，Q_data統計實做實算與總額結算項次紀錄，將合併欄位資料進行分割wrap(width=20)後Q list，加入倒T_list是2重list)，R_data第1次出現

檔案名稱3-1:Q=wrap(Q_data.loc[i,'合併'], width=21) #Q list 修改22個字20240419 修改22個字20240423，cjk_textwrap，Q_data複製P_data並將合併欄位資料進行分割，統計實做實算與總額結算項次紀錄。
檔案名稱3-2，3-3:將監造單位意見 填入序號1，現狀可保留下來，希望將手動輸入"/"也可以保留，英文字部分計算上比外觀多，看起來字數太少。中文字切割在一定長度，20。R_data出現，將R_data增加欄位，方便回復和P_data的排列方式。Q_data 是變動項次(分辨是否要跳行) 序號(方便比較項次) 序號1(合併之次項數量) R_data 是分割後散開資料=> STEP3考慮用轉矩陣後依各項len的長度做延續排列成1欄 STEP4新建立T_data將R_data填入項次及其他同一列資料回填同位置。

檔案名稱3-1，原始檔案背景格式已被覆蓋，+MDL1350009_大1_1131105.xlsx為臨時格式，執行時有ERROR。
檔案名稱3-2，原始檔案背景格式，請參考MDL1350005(從A欄).xls。
尋找舊檔案匯入也是有問題:
P_data.dtypes
Q_data.dtypes
ERROR CODE
TypeError                                 Traceback (most recent call last)
/usr/local/lib/python3.10/dist-packages/IPython/core/formatters.py in __call__(self, obj)
    339                 pass
    340             else:
--> 341                 return printer(obj)
    342             # Finally look for special method names
    343             method = get_real_method(obj, self.print_method)

/usr/local/lib/python3.10/dist-packages/google/colab/_reprs.py in _pandas_series_to_html(series)
    216     """Renders a pandas Series as a DataFrame HTML table with a dtype label."""
    217     series_as_table_html = series.to_frame()._repr_html_()  # pylint: disable=protected-access
--> 218     series_as_table_html += f'<br><label><b>dtype:</b> {series.dtype}</label>'
    219     return series_as_table_html
    220 

TypeError: unsupported operand type(s) for +=: 'NoneType' and 'str'
#3-2Bpandas_to_SQL(暫停Dtype指令)@20241106"+MDL1350009_大1_1131105.xlsx"



檔案名稱3-3，原始檔案背景格式，請參考MDL1350005(從A欄).xls。
檔案名稱3-4，原始檔案背景格式，MDL1350003_詢價單_#5_0518(密).xls
檔案名稱3-4，可使用在##STEP1。
檔案名稱3-5，可使用在請注意調整為標準欄位，最後1欄統計時，會把項次'0'刪除，手動填入'/'方便未來分項目的。

##Step2 (處理可上網報價單檔案，合併成1列，方便傳入SQL，因三種不同跳行情況...)
#4-1，4-2，4-3，4-4B為暫時不使用。
#4-5A處理 見#4-5D
#4-5B處理 建立跳行_來源檔項次為文字'0'，"MDL1350003_0.96"文字'0'，否可填入指定文字而不是數字0，功能性不大，項次欄空白已經在前端python中填入特定變數，#pattern = re.compile(r'^\([0-9]\)|^\([a-z]\)|^工資部=>。
#4-5C處理 建立跳行_來源檔項次為文字''，"MDL1350005"文字''，否可填入指定文字而不是數字0，功能性不大，項次欄空白已經在前端python中填入特定變數
#4-5D處理 建立跳行_來源檔項次為Nan''，"MDL0750002"，指定文字'0'，同#4-5A處理 建立跳行_來源檔項次為Nan，"MDL1350001_SQL"文字'0'
Step2#

##Step3
細項編碼有鍵入的
MDL1350005_主要的_廠商有報價
項次與工作項目 兩邊不一致需要整理並為階層放入特別符號”///”(計價用)，”//”(群組用，多1欄表示從哪開始到結束)，，”/-”，(散開的工作項目)並分別存入calc與CSV檔案格式。
MDL1050001_部分_決標價格，MDL0850001_部分_決標價格，MDL1350001_部分_決標價格，MDL1350005_全部有細項編碼，MDL1350003_沒有(因為是總額結算具多)。

#5-1,jieba範例練習，原設定:繁體字內存小small，繁體字內存大big。userdict.txt 提供給jieba使用，增加判斷正確性，P_data.loc[:,'字串']=" " #另一種寫法。
#5-1A,jieba.analyse #據 TF-IDF 算法來找出句子的關鍵字，用不到。
#5-2，"SQL_工作項目及說明_總額計算_MDL1350005 (資料清理)_0919.xls=>有將[受詞   形容詞  群組結束(//)  RIGHT(W2,7)]等加入完成。(1)the '\n'要清除後，再行cut(2)userdict.ver1.0=>版本由MDL1350005，userdict.ver2.0=>分版本由MDL1350009，同一詞語建議長詞語需要統一=>userdict，已在EXCEL內特定範圍，移動排列後刪除空白及重複後，寫入TEXT檔 (3)加入stop_words.txt=>功能性不強 (4)詞性為不知道的資料結構，用不到。
#5-2a，for j in range(27,31):P_data.loc[i,"關鍵字(主詞)動詞受詞形容詞"])(P_data.loc[i,"關鍵字(主詞)動詞受詞形容詞"])(P_data.loc[i,"關鍵字(主詞)動詞受詞形容詞"])
#5-3，5-4，5-4A，5-4B，5-4C"檢查_jieba_在字串.xlsx"，檢查兩個串列是否有含相同的元素，但不在乎順序是否重複時(需要全部元素都一樣)，可利用型別set(得到不重複元素的組合)，非常便利。 不能用在=>比對細項編碼是否出現在新出現工項中。 集合有嚴謹的數學定義，python也以運算子和方法支援集合的數學運算，包括聯集、交集、差集、想檢查兩個串列是否有含相同的元素，但不在乎順序是否重複時(需要全部元素都一樣)，可利用型別set(得到不重複元素的組合)，非常便利。 不能直接用==比對細項編碼是否出現在新出現工項中。 但可以用<= issubset 集合有嚴謹的數學定義，python也以運算子和方法支援集合的數學運算，包括聯集、交集、差集、對稱差集、以及子集合、超集合的判斷。user_stop_dict=>準備要處理檔案的list，MDL1350005_全部有細項編碼，可作為自己測試自己。寫入list第1位置有困難，要建立多個dict去關連到價格上。利用迴圈去比對(1)細項編碼與(2)M13-05(1)user_dict.items()與20241001B.json(2)user_stop_dict.items()與20241001C.json。True要寫入其他dict，user_stop_dict還需要進行其他比對，fail會有相乘效應，有困難。'有配對的細項編碼',len(result),'是否與本次標的',len(user_stop_dict),'含利潤與營業稅等,項次一致',len(python_dict),'現有資料庫的數量不含利潤與營業稅等')。

#5-4D，在OpenDocument中xlsx與xls，針對函數值不會進行計算，失敗改xlsm MDL1350005與下一次目標(大1MDL0750002，MDL1350009)
#5-4E，正常版本。使用userdict_later.json，使用stop_dict_later.json，生成user_dict.json(可提供VER2.0)

#6_EXCEL_關鍵字_比較並寫入新的user_dict在其他檔案.ipynb=>STEP4.2_SQL_工作項目及說明_總額計算(0815)_M810-01.xlsx=>(永安MDL1050001，MDL1350005)，pattern = re.compile(r'^/|^壹|^貳|^參|^肆|^伍') #設定去除大項的說明，ok_result_M810-01.json，fail_result_M810-01.txt，失敗 [1.0, 1.2, 2.0, 2.1, 2.11, 2.12, 2.8, 2.9, 3.0, 3.2, 3.3, 3.4, 3.5, 4.0, 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 5.0, 5.1, 5.2, 5.3, 6.0, 6.2, 6.4, 6.5, 6.7, 7.0, 7.1, 7.2, 7.3, 8.0, 8.2, 8.5, 8.6, 9.0, 9.1, 9.3, 9.4, 9.5, 9.6, 9.7, 10.0, 10.1, 10.11, 10.12, 10.13, 10.14, 10.15, 10.16, 10.17, 10.24, 10.25, 10.26, 10.27, 10.28, 10.29, 10.3, 10.31, 10.34, 10.35, 10.4, 10.5, 10.6, 10.7, 10.8, 10.9, 11.0, 11.1, 11.11, 11.12, 11.13, 11.14, 11.15, 11.16, 11.17, 11.18, 11.19, 11.2, 11.21, 11.22, 11.23, 11.24, 11.25, 11.26, 11.27, 11.28, 11.29, 11.3, 11.31, 11.32, 11.33, 11.34, 11.35, 11.36, 11.37, 11.38, 11.39, 11.4, 11.41, 11.42, 11.43, 11.44, 11.45, 11.5, 11.6, 11.7, 11.8, 11.9, 12.0, 12.1, 12.2, 12.3, 12.4, 12.5, 12.6, 12.7, 12.8, 12.9, 13.0, 13.2, 14.0, 14.1, 14.2, 14.3, 14.4, 14.5, 15.0, 15.1, 16.0, 16.1, 16.2, 16.3, 17.1, 17.2, 17.3, 18.0, 18.11, 18.13, 18.14, 18.15, 18.16, 18.17, 18.18, 18.19, 18.21, 18.3, 18.4, 18.5, 18.7, 18.8, 18.9, 19.0, 19.2, 20.0, 20.1, 20.2, 20.3, 20.4, 20.5, 20.6, 20.7, 20.8, 20.9, 21.0, 21.1, 21.2, 21.3, 21.4, 21.5]
有配對的細項編碼 37 是否與本次標的 219 含利潤與營業稅等,項次一致 179 現有資料庫的數量不含利潤與營業稅等

#7-1EXCEL資料copy到另一個EXCEL(最後1頁手動)，
STEP4_契約單價自動比對_大林1號，Base與python_文字挖掘關鍵字(雲端內的檔案夾)，
使用目的 不清楚。

Step3##

其他:AIS 是利用realtek rtl2832u 的SDR,後端軟體處裡功能，非U120 數位電視棒所用DVB-T的IC，此項功能應用對象非一班民眾。
建議傳統熱水器與瓦斯爐(非接觸紅外線溫度計)與鋁門窗結構監控(非接觸位移計)，
