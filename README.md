# ファイル名: sysmon_network_view.py

# Sysmon イベントログ（Text,XML）からネットワークランキングもしくは宛先マップを描画するツール

## 前提条件
- SysmonイベントログからXML化したファイルがあること
    - 参考）"wevtutil qe Microsoft-Windows-Sysmon/Operational /f:Xml > ./sysmon.xml"
    - 参考）wevtutil qe Microsoft-Windows-Sysmon/Operational /q:"*[System[(EventID=3 or EventID=22)]]" /f:XML > sysmon.xml
    - 注意）sysmon.xml は、utf-16形式なので注意
- MaxMind社の以下のGeoIpデータベースをローカルに保存していること
    - 参考）'GeoLite2-ASN.mmdb' 及び 'GeoLite2-City.mmdb'

## 動作概要
- Sysmonイベントログファイル（sysmon.xml）及びGeoIPデータベース（GeoLite2-ASN.mmdb',
'GeoLite2-City.mmdb')が指定されていない場合、カレントもしくはホームディレクトリ配下を検索
    - XMLファイルを読み込みEVENTID=3(NetworkConnect)の情報をpandasのデータフレームに変換
        - 参考）XMLの処理の参考例: https://qiita.com/sino20023/items/0314438d397240e56576
        - 参考）sysmon のデータ構造: https://qiita.com/kasanma3104/items/6971d3e7d09de134da37
- XMLファイルを読み込みEVENTID=22(DNS Query)の情報をpandasのデータフレームに変換
- DNSのデータからdomain情報を抽出
- pandas データフレームを基にtopランキングを表示（デフォルトはIPのランキング、任意に指定可能）
- foliumにて世界地図にアクセス先を表示(-m 指定の場合のみ実行)
- pandas データフレーム（sysmonデータ、topnランキング）、世界地図htmlを保存
- オプションでログに不審なexeファイル、アクセス先を追加
