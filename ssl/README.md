# 自簽憑證
  ##### 1. generate private key
    openssl genrsa -des3 -out customCA.key 2048
  
  ##### 2. generate root certificate
    openssl req -x509 -new -nodes -key customCA.key -sha256 -days 1825 -out customCA.pem
   
   輸入指令後，會要求填入下列資訊：
   >> Country Name (2 letter code) []:TW
   >>  State or Province Name (full name) []:Taipei City
   >>  Locality Name (eg, city) []:TaipetCity
   >>  Organization Name (eg, company) []:Ron company
   >>  Organizational Unit Name (eg, section) []:
   >>  Common Name (eg, fully qualified host name) []:ron.custom.com
   >>  Email Address []:aa365321@gmail.com

  ##### 3. 上面兩步驟後就會得到
  Private key (customCA.key) 與 Root certificate (customCA.pem)