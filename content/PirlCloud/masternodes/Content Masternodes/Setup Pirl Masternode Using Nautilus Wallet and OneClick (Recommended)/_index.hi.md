---
title: सेटअप कंटेंट मास्टरनोड
weight: 1
pre: "<b>1. </b>"
chapter: true
---

{{< imagesurlsheaders "images_headers/Masternodes.png" >}}


- [अवलोकन](#overview)
- [पूर्वापेक्षा](#prerequisites)
- [पोस्याडॉन वॉलेट आइडेंटिटी वेरिफिकेशन](#poseidon-wallet-identity-verification)
- [नॉटिलस कॉन्ट्रैक्ट निष्पादन](#nautilus-contract-execution)
- [CentOS Linux सर्वर बनाएं / लॉन्च करें](#create-launch-centos-linux-server)
- [पोस्याडॉन में मास्टर्नोड बनाएं](#create-masternode-in-poseidon)
- [एक-क्लिक मास्टरनोड सेटअप](#one-click-masternode-setup)
- [निगरानी (मोनेटरींग)](#monitoring)




## अवलोकन

प्रिल मास्टरनोड को चलाने के लिए एक वर्चुअल प्राइवेट सर्वर (VPS) की आवश्यकता होती है और यह सर्वर को एक स्टैटीक पब्लिक आईपी एड्रेस होना चाहिऐ जो की डायरेक्ट इंटरफेस को असाइन चाहिए।  
__*NAT एड्रेस समर्थित नहीं है।*__  
सर्वर पर केवल प्रिल ही चलाए, किसी भी अन्य नोड्स या कुछ और एक संघर्ष का कारण होगा!

यह मार्गदर्शिका एक-क्लिक मास्टरनोड सेटअप सुविधा का उपयोग करती है।
यह पोस्याडॉन सुविधा स्वचालित रूप से आपके CentOS 7 linux सर्वर को कॉन्फ़िगर करके प्रिल मास्टरनोड बनाती है।
अपडेट अपने आप इंस्टॉल हो जाएंगे।  
आपको बस आपके सर्वर की निगरानी करके यह सुनिश्चित करना है कि वह चालू है।  
अगर आपका सर्वर ऑफ़लाइन चले जाता है तो बस सर्वर को रीबूट करे।  


## पूर्वापेक्षा

* **न्यूनतम 4जीबी कुल OS रैम वाला VPS (अधिक अनुशंसित है), मास्टरनोड को चलाने के लिए पर्याप्त भंडारण (न्यूनतम 20जीबी, अनुशंसित 60जीबी +), एक स्टैटीक पब्लिक आईपी एड्रेस चाहिऐ जो की डायरेक्ट इंटरफेस को असाइन चाहिए। NAT एड्रेस समर्थित नहीं है।**

 - आधिकारिक न्यूनतम आवश्यकताएं हैं: 4 जीबी रैम, 20 जीबी स्पेस, 3 टीबी ट्रान्सर्फर, सार्वजनिक IPv4 आईपी।

 - एक बार जब आप अपना VPS ऑर्डर कर देते हैं, तो आपको उसके क्रेडेंशियल प्राप्त होगे। आगे का सबसे आसान तरीका ये है की आप ये VPS केवल प्रिल के लिए इस्तमाल करे और पोस्याडॉन को अपनी रूट क्रेडेंशियल दे ताकि वह आपके VPS को प्रबंधित और अपडेट कर सके।

* **पोस्याडॉन पर एक अकाउंट [https://poseidon.pirl.io](https://poseidon.pirl.io)**
 - निमनलिखीत पर जाऐ https://poseidon.pirl.io और अकाउंट के लिए पंजीकरण करें। ध्यान रखें कि आप अपने **युझर नेम** के साथ लॉग इन करेंगे और ईमेल के साथ नहीं।
* **नॉटिलस वैलेट**
 - नॉटिलस, प्रिल का आधिकारिक डेस्कटॉप वॉलेट है। आपको प्रिल मास्टरनोड को चलाने के लिए आवश्यक स्मार्ट कॉन्ट्रैक्ट से “रजिस्टर नोड“ को जोड़ने और निष्पादित करने के लिए इसकी आवश्यकता होगी।  
 आप अपने प्रिल वॉलेट को बनाने के लिए डेस्कटॉप वॉलेट का उपयोग कर सकते है। [Downloads Nautilus]({{< ref "/Downloads" >}}) या आप वेब वॉलेट का उपयोग निमनलिखीत से कर सकते है: https://wallet.pirl.io/.    
 - जो भी विधि आप अपने वॉलेट को बनाने के लिए चुनते हैं, हमेशा सुनिश्चित करें कि आप अपनी यूटीसी फ़ाइल और पासवर्ड को सेव करे!  
 - चेतावनीः यूटीसी फ़ाइल का पासवर्ड पुनर्प्राप्त नहीं किया जा सकता, सुनिश्चित करें कि आप इसे याद रखें या इसे लिख लें!

* **कंटेंट मास्टरनोड के लिए आपके वॉलेट में 10,001 प्रिल उपलब्ध चाहिए।**  
 - इसका दुसरा कोई और तरीका नही है, आपको किसी तरह से दस हज़ार प्रिल को एक वॉलेट में लाना होगा।
 - 1 या 0.5 गैस के लिए ताकी कॉन्ट्रैक्ट से बातचित हो सके।
 - आप यहां उपलब्ध आधिकारिक पूल में से किसी एक का उपयोग करके प्रिल को माईन कर सकते हैं: https://pirl.io/en/pools/.
 - आप प्रिल को एक्सचेंज पर खरीद सकते हैं। मेरा सुझाव है https://www.stex.com एक सुरक्षित और विश्वसनीय एक्सचेंज के रूप में।


 ## पोस्याडॉन वॉलेट आइडेंटिटी वेरिफिकेशन

 - निमनलिखीत पर जाऐ https://poseidon.pirl.io/ और लॉगिन करें -> पोस्याडॉन वॉलेट पर नेविगेट करें https://poseidon.pirl.io/dashboard/accounting/wallet/ और अपने अद्वितीय पोस्याडॉन वॉलेट एड्रेस को कॉपी करे।

 - अपना नॉटिलस या वेब वॉलेट खोलें (जो भी आप ने चुना हो और जहा आपका स्टेक है) और पिछले चरण में आपके द्वारा कॉपी किए गए पोस्याडॉन वॉलेट एड्रेस पर 0.5 प्रिल भेजें।

 - एक बार जब आप वेरीफीकेशन ट्रांसेक्शन भेज देते हैं, तो नेविगेट करें https://explorer.pirl.network/ और सर्च बार में अपना वॉलेट एड्रेस पेस्ट करें। यह सर्च आपके वॉलेट और उसे संबंधित ट्रांसेक्शन को प्रदर्शित करेगी -> अपने वॉलेट से अपने पोस्याडॉन वॉलेट में 0.5 प्रिल का अंतिम ट्रांसेक्शन ढूंढें।

 - ट्रांसेक्शन ब्लॉक की पहली पंक्ति ट्रांसेक्शन हैश प्रदर्शित करती है। बाद में सेटअप के दौरान आपको अपने मास्टरनोड और आपके पोस्याडॉन वॉलेट के बीच लिंक को सत्यापित करने के लिए इस ट्रांसेक्शन हैश की आवश्यकता होगी।


 ![](https://i.imgur.com/tAWr8Ua.png)

 **नोटः सेटअप के लिए इस एड्रेस पर 1 या .5 प्रिल से ज्यादा ना भेजें, यह वह एड्रेस नहीं है जिसपे आपको बाद में 10 हजार प्रिल भेजने है।**


 यदि आप नॉटिलस वॉलेट का उपयोग कर रहे हैं, तो आप अंतिम भेजे गए ट्रांसेक्शन पर एक बार क्लिक कर सकते हैं और आप ट्रांजेक्शन हैश देख सकते हैं (बाद में मास्टरनोडे TX हैश क्षेत्र में पूछा जायेगा):

 {{< imagesurlsheaders "cloud/txnautilus.png" >}}





 ## नॉटिलस कॉन्ट्रैक्ट निष्पादन

 - **नॉटिलस को खोलें** और ऊपरी दाएं कोने पर स्थित **कॉन्ट्रैक्ट** टैब पर जाएं।

 ![](https://cdn-images-1.medium.com/max/1600/0*OW_7W9P_u0k7ZdmZ.png)

 - वहां जाने के बाद **वॉच कॉन्ट्रैक्ट** बटन पर क्लिक करें।

 ![](https://cdn-images-1.medium.com/max/1600/0*wZbZlfAdjrUuhr53.png)

 **इस फॉर्म में आपको भरना होगा:**


**कंटेंट मास्टरनोड:** के लिये **कॉन्ट्रैक्ट एड्रेस** मे भरें `0x6c042141C302C354509d2bff30EEFDEF24dE1047`.
**’कॉन्ट्रैक्ट नेम** इस कॉन्ट्रैक्ट को एक नाम दे नाम आपके अनुसार कुछ भी हो सकता है
और अंत में **JSON इंटरफ़ेस क्षेत्र** निमनलिखित भर देः


```
[{"constant":false,"inputs":[],"name":"nodeRegistration","outputs":[{"name":"paid","type":"bool"}],"payable":true,"stateMutability":"payable","type":"function"},{"constant":true,"inputs":[{"name":"_pirlAddress","type":"address"}],"name":"getNodeAddress","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"moderators","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"nodes","outputs":[{"name":"pirlAddress","type":"address"},{"name":"nodeStake","type":"uint256"},{"name":"nodeHash","type":"bytes20"},{"name":"stakeLocked","type":"bool"},{"name":"nodeEnabled","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[],"name":"disableNodeRegistration","outputs":[{"name":"disabled","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"nodeCost","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"_pirlAddress","type":"address"}],"name":"getStakeLockedStatus","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"nodeCount","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_admin","type":"address"}],"name":"setAdmin","outputs":[{"name":"set","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"owner","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[],"name":"enableNode","outputs":[{"name":"enabled","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"nodeRegistrationEnabled","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[],"name":"disableNode","outputs":[{"name":"disabled","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[],"name":"withdrawStake","outputs":[{"name":"withdrawn","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"","type":"uint256"}],"name":"nodeAddresses","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"_pirlAddress","type":"address"}],"name":"getNodeEnabledStatus","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"_pirlAddress","type":"address"}],"name":"getNodeStake","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[],"name":"enableNodeRegistration","outputs":[{"name":"enabled","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"_pirlAddress","type":"address"}],"name":"getNodeHash","outputs":[{"name":"","type":"bytes20"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"nodeFee","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"admin","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"inputs":[],"payable":false,"stateMutability":"nonpayable","type":"constructor"},{"payable":true,"stateMutability":"payable","type":"fallback"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_pirlAddress","type":"address"},{"indexed":true,"name":"_nodeHash","type":"bytes20"},{"indexed":true,"name":"_nodeRegistered","type":"bool"},{"indexed":false,"name":"_dateRegistered","type":"uint256"}],"name":"MasterNodeRegistered","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_pirlAddress","type":"address"},{"indexed":true,"name":"_nodeHash","type":"bytes20"},{"indexed":true,"name":"_nodeDisabled","type":"bool"},{"indexed":false,"name":"_dateDisabled","type":"uint256"}],"name":"MasterNodeDisabled","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_pirlAddress","type":"address"},{"indexed":true,"name":"_nodeHash","type":"bytes20"},{"indexed":true,"name":"_nodeEnabled","type":"bool"},{"indexed":false,"name":"_dateEnabled","type":"uint256"}],"name":"MasterNodeEnabled","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_pirlAddress","type":"address"},{"indexed":true,"name":"_nodeHash","type":"bytes20"},{"indexed":true,"name":"_nodePaid","type":"bool"},{"indexed":false,"name":"_datePaid","type":"uint256"}],"name":"MasterNodeRewarded","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_pirlAddress","type":"address"},{"indexed":true,"name":"_nodeHash","type":"bytes20"},{"indexed":true,"name":"_stakeWithdrawn","type":"bool"},{"indexed":false,"name":"_dateWithdrawn","type":"uint256"}],"name":"StakeWithdrawn","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_invoker","type":"address"},{"indexed":false,"name":"_dateEnabled","type":"uint256"},{"indexed":true,"name":"_registrationEnabled","type":"bool"}],"name":"MasterNodeRegistrationEnabled","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_invoker","type":"address"},{"indexed":false,"name":"_dateDisabled","type":"uint256"},{"indexed":true,"name":"_registrationDisabled","type":"bool"}],"name":"MasterNodeRegistrationDisabled","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_invoker","type":"address"},{"indexed":true,"name":"_admin","type":"address"},{"indexed":true,"name":"_adminSet","type":"bool"}],"name":"SetAdmin","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_invoker","type":"address"},{"indexed":true,"name":"_newOwner","type":"address"},{"indexed":true,"name":"_ownerChanged","type":"bool"}],"name":"TransferOwnership","type":"event"}]
```


- कॉन्ट्रैक्ट अनुभाग में -> उस कॉन्ट्रैक्ट का चयन करें जिसे आपने अभी जोड़ा है और आप उपलब्ध कार्यों को ड्रॉपडाउन मेनू में देखेंगे जोकी दाईं ओर **राइट टू कॉन्ट्रैक्ट** शीर्षक के तहत है।  

- उपलब्ध कार्यों के तहत **नोड रजिस्ट्रेशन** का चयन करें और कंटेंट मास्टरनोड के लिए अपने 10,000 प्रिल वाले वैलेट का चयन करें।  
फिर उसके निचे कंटेंट मास्टरनोड के कॉन्ट्रैक्ट में स्टेक भेजने के लिए **10,000 प्रिल** भरें।


{{< imagesurlsheaders "cloud/10k.png" >}}  


- वो वैलेट का चयन करने के बाद जिसमें स्टेक है और **नोड रजिस्ट्रेशन** फ़ंक्शन का चयन करने के बाद **एकझिक्युट** पर क्लिक करें। अपने  **UTC फ़ाइल पासवर्ड को भरें** और सुनिश्चित करें कि आप ट्रांसेक्शन के लिए **कम से कम 121,000 गैस** प्रदान कर रहे हैं।  

यह कुछ कॉफी या चाय प्राप्त करने का अच्छा समय है । 3-5 मिनट के बीच सब कुछ सिंक हो जाना चाहिए।  

## CentOS Linux सर्वर बनाएं / लॉन्च करें

सत्यापित करें कि सर्वर उपयुक्त विशिष्टताओं को पूरा करता है:  
[पूर्वापेक्षा](#prerequisites)  

- यदि आप **वन-क्लिक मास्टरनोड सेटअप** का उपयोग करने की योजना बनाते हैं, तो सर्वर को CentOS 7 लिनक्स वितरण को चलाना होगा।  

- सर्वर के स्टैटीक पब्लिक आईपी एड्रेस के साथ-साथ रूट पासवर्ड का रिकॉर्ड।  

नोटः हम रूट क्रेडेंशियल्स सुनिश्चित करने के लिए एक बार उस सर्वर में लॉग इन करने की सलाह देते हैं। इसके बाद सर्वर पर कोई अन्य कार्रवाई करना आवश्यक नहीं है। वास्तव में, आप किसी भी अन्य समायोजन नहीं करते हैं तो ज्यादा बेहतर होगा।



## पोस्याडॉन में मास्टरनोड बनाएं

- पोस्याडॉन में लॉगिन करें और उस पृष्ठ पर जाएँ जो मास्टरनोड जोड़ता है :   https://poseidon.pirl.io/dashboard/masternodes/   
- पृष्ठ के दाईं ओर स्थित प्लस बटन दबाएं:  


![](https://i.imgur.com/N0rUi7z.jpg)  

- क्रिएट मास्टरनोड पॉप-अप दिखाई देगा  


{{< imagesurlsheaders "cloud/Create_content_Masternode_Record_in_Poseidon.png" >}}  



- अपनी पसंद का नाम मास्टरनोड को दें।  

- मास्टरनोड वॉलेट आईडी आपके नॉटिलस वॉलेट का एड्रेस है, जिसमें वर्तमान में 10,000 प्रिल हैं।  

- मास्टरनोड TX हैश - **पोस्याडॉन वॉलेट आइडेंटिटी वेरिफिकेशन** चरण में जो ट्रांजेक्शन हैश हमने सेव किया था उसका उपयोग करे।

**ऊपर दिए गए स्क्रीनशॉट के नीचले भाग मे आपको चुनना होगा कंटेंट मास्टरनोड (10K स्टेक)**

हिट  **सेव चेनजेस** और फिर आपको अगली स्क्रीन दिखाई देगी।


{{< imagesurlsheaders "cloud/one_click_setup.PNG" >}}




## एक-क्लिक मास्टरनोड सेटअप

- आगे बढ़ने से पहले सुनिश्चित करें कि आप के पास स्टैटीक पब्लिक आईपी एड्रेस और रूट क्रेडेंशियल्स हैं।  



{{< imagesurlsheaders "cloud/one_click_setup.PNG" >}}  



- निमनलिखीत फ़ील्ड्स को पूरा करें -> SSH का डिफ़ॉल्ट पोर्ट हैः 22 । **सेव चेनजेस**  पर क्लिक करे फिर आपको अगली स्क्रीन दिखाई देगी।  


{{< imagesurlsheaders "cloud/Done.PNG" >}}  

- **माई मास्टरनोड्स** स्क्रीन पर लौटने के बाद, यह निरीक्षण करें कि मास्टरनोड्स **म्यानेज्ड बाय पोस्याडॉन** फ़ील्ड पर सेट है।  

{{< imagesurlsheaders "cloud/managed.jpg" >}}  


- कृपया प्रक्रिया को पूरा करने के लिए 30 मिनट का समय दें। आप स्थिति की निगरानी के लिए  **डिटेलस** बटन पर क्लिक कर सकते हैं।  

- ब्लॉकचेन के साथ सिंक्रनाइज़ होने वाली मास्टर्नोड प्रक्रिया देखें:  
```
journalctl -f
```

- एक बार जब निमनलिखीत जैसे संदेश प्रदर्शित होने लगे तो आपका मास्टर्नोड अब सिंक्रनाइज़ हो गया है और नेटवर्क में योगदान कर रहा है।


{{< imagesurlsheaders "cloud/vps.jpg" >}}  




## निगरानी (मोनेटरींग)

हम सर्वर पर एैकटीव ऐकसेस को प्रोत्साहित नहीं करते हैं। फिर भी आप स्थिति की जाँच करना चाहते हैं, तो अपने सर्वर में लॉग इन करें और निम्नलिखित आदेश जारी करें:  
```
journalctl -f
```
आपका मास्टरनोड नेटवर्क मे योगदान दे रहा है यदी आपकी स्क्रीन पर इस तरह दिख रहा हैः  



{{< imagesurlsheaders "cloud/vps.jpg" >}}  



अपने मास्टर्नोड की स्थिति की निगरानी करने के लिए पोस्याडॉन मास्टरनोड विवरण पृष्ठ पर  🔍 क्लिक करे।   
आपका नोड निम्नानुसार दिखाई देना चाहिएः  


{{< imagesurlsheaders "cloud/detailsmn.png" >}}  





---
Author(s):  


The Pirl Team  


Contributor(s):  


@Dptelecom  
