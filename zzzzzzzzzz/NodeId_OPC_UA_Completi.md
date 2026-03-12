# Codice Function Nodes OPC UA - Learning Factory 4.0

**Data estrazione**: 11 Febbraio 2026  
**File sorgente**: flows-6.json  
**Function nodes con OPC UA**: 131

---

## Indice Function Nodes

1. [read DSI state](#read-dsi-state)
2. [declare values](#declare-values)
3. [read DSO state](#read-dso-state)
4. [declare values](#declare-values)
5. [read HBW state](#read-hbw-state)
6. [declare values](#declare-values)
7. [read MPO state](#read-mpo-state)
8. [declare values](#declare-values)
9. [read SLD state](#read-sld-state)
10. [declare values](#declare-values)
11. [read VGR state](#read-vgr-state)
12. [declare values](#declare-values)
13. [value to write](#value-to-write)
14. [values to read](#values-to-read)
15. [values to write](#values-to-write)
16. [values to write](#values-to-write)
17. [read NFC data from PLC](#read-nfc-data-from-plc)
18. [declare values](#declare-values)
19. [values to write](#values-to-write)
20. [values to read](#values-to-read)
21. [declare values](#declare-values)
22. [read data from PLC](#read-data-from-plc)
23. [declare values](#declare-values)
24. [values to write](#values-to-write)
25. [read Order state](#read-order-state)
26. [declare values](#declare-values)
27. [value to write](#value-to-write)
28. [values to write](#values-to-write)
29. [values to write](#values-to-write)
30. [values to read](#values-to-read)
31. [declare values](#declare-values)
32. [declare values](#declare-values)
33. [declare values](#declare-values)
34. [declare values](#declare-values)
35. [read config data from PLC](#read-config-data-from-plc)
36. [declare values](#declare-values)
37. [values to read](#values-to-read)
38. [declare values](#declare-values)
39. [declare values](#declare-values)
40. [declare values](#declare-values)
41. [declare values](#declare-values)
42. [declare values](#declare-values)
43. [read config data from PLC](#read-config-data-from-plc)
44. [declare values](#declare-values)
45. [declare values](#declare-values)
46. [declare values](#declare-values)
47. [declare values](#declare-values)
48. [declare values](#declare-values)
49. [declare values](#declare-values)
50. [declare values](#declare-values)
51. [declare values](#declare-values)
52. [declare values](#declare-values)
53. [declare values](#declare-values)
54. [declare values](#declare-values)
55. [declare values](#declare-values)
56. [declare values](#declare-values)
57. [declare values](#declare-values)
58. [declare values](#declare-values)
59. [declare values](#declare-values)
60. [read config data from PLC](#read-config-data-from-plc)
61. [declare values](#declare-values)
62. [declare values](#declare-values)
63. [declare values](#declare-values)
64. [declare values](#declare-values)
65. [declare values](#declare-values)
66. [declare values](#declare-values)
67. [declare values](#declare-values)
68. [declare values](#declare-values)
69. [declare values](#declare-values)
70. [declare values](#declare-values)
71. [declare values](#declare-values)
72. [declare values](#declare-values)
73. [declare values](#declare-values)
74. [declare values](#declare-values)
75. [declare values](#declare-values)
76. [declare values](#declare-values)
77. [declare values](#declare-values)
78. [declare values](#declare-values)
79. [declare values](#declare-values)
80. [declare values](#declare-values)
81. [declare values](#declare-values)
82. [declare values](#declare-values)
83. [declare values](#declare-values)
84. [values to read](#values-to-read)
85. [declare values](#declare-values)
86. [declare values](#declare-values)
87. [read config data from PLC](#read-config-data-from-plc)
88. [declare values](#declare-values)
89. [declare values](#declare-values)
90. [declare values](#declare-values)
91. [declare values](#declare-values)
92. [declare values](#declare-values)
93. [values to read](#values-to-read)
94. [declare values](#declare-values)
95. [declare values](#declare-values)
96. [declare values](#declare-values)
97. [declare values](#declare-values)
98. [declare values](#declare-values)
99. [declare values](#declare-values)
100. [declare values](#declare-values)
101. [read config data from PLC](#read-config-data-from-plc)
102. [declare values](#declare-values)
103. [declare values](#declare-values)
104. [declare values](#declare-values)
105. [declare values](#declare-values)
106. [declare values](#declare-values)
107. [declare values](#declare-values)
108. [declare values](#declare-values)
109. [values to read](#values-to-read)
110. [declare values](#declare-values)
111. [declare values](#declare-values)
112. [read config data from PLC](#read-config-data-from-plc)
113. [declare values](#declare-values)
114. [declare values](#declare-values)
115. [declare values](#declare-values)
116. [declare values](#declare-values)
117. [declare values](#declare-values)
118. [declare values](#declare-values)
119. [declare values](#declare-values)
120. [declare values](#declare-values)
121. [declare values](#declare-values)
122. [values to read](#values-to-read)
123. [declare values](#declare-values)
124. [read config data from PLC](#read-config-data-from-plc)
125. [declare values](#declare-values)
126. [values to read](#values-to-read)
127. [prepare HBW order](#prepare-hbw-order)
128. [Select SSC Position](#select-ssc-position)
129. [Start Positioning](#start-positioning)
130. [write PTU setpoints](#write-ptu-setpoints)
131. [start positioning](#start-positioning)

---

## 1. read DSI state

**Node ID**: `5fdaf37d.b1ac9c`

### Codice JavaScript

```javascript


msg.nodetype = "inject";
msg.injectType = "read";

//Items for state DSI
msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSI"."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSI"."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSI"."s_description"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSI"."s_station"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSI"."s_target"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSI"."x_active"',
        "datatypeName":'Bool'},
        ]

return msg;
```

---

## 2. declare values

**Node ID**: `a46a60e3.1696c`

### Codice JavaScript

```javascript
var ts = new Date().toISOString();

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSI"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.payload["ts"] = ts;
msg.valuesToWrite = [ts];

return msg;
```

---

## 3. read DSO state

**Node ID**: `14047040.277b4`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

//Items for state DSO
msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSO"."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSO"."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSO"."s_description"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSO"."s_station"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSO"."s_target"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSO"."x_active"',
        "datatypeName":'Bool'},
        ]

return msg;
```

---

## 4. declare values

**Node ID**: `725f5e6d.8fd61`

### Codice JavaScript

```javascript
var ts = new Date().toISOString();

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_DSO"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.payload["ts"] = ts;
msg.valuesToWrite = [ts];

return msg;
```

---

## 5. read HBW state

**Node ID**: `84a4753b.c48e5`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

//Items for state HBW
msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_description"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_station"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."s_target"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."x_active"',
        "datatypeName":'Bool'},
        ]

return msg;
```

---

## 6. declare values

**Node ID**: `d0d1fa6e.2b991`

### Codice JavaScript

```javascript
var ts = new Date().toISOString();

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_HBW"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.payload["ts"] = ts;
msg.valuesToWrite = [ts];

return msg;
```

---

## 7. read MPO state

**Node ID**: `ba6cb267.b075e8`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

//Items for state MPO
msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_MPO"."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_MPO"."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_MPO"."s_description"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_MPO"."s_station"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_MPO"."s_target"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_MPO"."x_active"',
        "datatypeName":'Bool'},
        ]

return msg;
```

---

## 8. declare values

**Node ID**: `e3aefda7.813c68`

### Codice JavaScript

```javascript
var ts = new Date().toISOString();

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_MPO"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.payload["ts"] = ts;
msg.valuesToWrite = [ts];

return msg;
```

---

## 9. read SLD state

**Node ID**: `1120b5a5.d9c70a`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

//Items for state MPO
msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_SLD"."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_SLD"."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_SLD"."s_description"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_SLD"."s_station"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_SLD"."s_target"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_SLD"."x_active"',
        "datatypeName":'Bool'},
        ]

return msg;
```

---

## 10. declare values

**Node ID**: `e4e1804b.7d72e`

### Codice JavaScript

```javascript
var ts = new Date().toISOString();

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_SLD"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.payload["ts"] = ts;
msg.valuesToWrite = [ts];

return msg;
```

---

## 11. read VGR state

**Node ID**: `acf217e4.bd543`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

//Items for state MPO
msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_VGR"."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_VGR"."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_VGR"."s_description"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_VGR"."s_station"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_VGR"."s_target"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_VGR"."x_active"',
        "datatypeName":'Bool'},
        ]

return msg;
```

---

## 12. declare values

**Node ID**: `54ad3cee.228c5c`

### Codice JavaScript

```javascript
var ts = new Date().toISOString();

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_VGR"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.payload["ts"] = ts;
msg.valuesToWrite = [ts];

return msg;
```

---

## 13. value to write

**Node ID**: `3c0bf468.2670ec`

### Codice JavaScript

```javascript


msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Publish"."ldt_AcknowledgeButton"',
        "datatypeName":'DateTime'},
        ]

msg.valuesToWrite = [
    msg.payload.ts
    ]

return msg;
```

---

## 14. values to read

**Node ID**: `45d2f19d.87a89`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."AlertMessage"."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."AlertMessage"."s_data"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."AlertMessage"."s_id"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."AlertMessage"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]


return msg;
```

---

## 15. values to write

**Node ID**: `4f4071ae.f69d18`

### Codice JavaScript

```javascript


msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Publish"."ActionButtonNFCModule"."s_cmd"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Publish"."ActionButtonNFCModule"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.valuesToWrite = [
    msg.payload.cmd,
    msg.payload.ts
    ]

return msg;
```

---

## 16. values to write

**Node ID**: `a7818371.2e3468`

### Codice JavaScript

```javascript
var i;


msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."Workpiece"."s_id"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."Workpiece"."s_state"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."Workpiece"."s_type"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[0]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[0]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[1]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[1]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[2]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[2]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[3]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[3]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[4]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[4]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[5]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[5]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[6]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[6]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[7]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[7]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[8]."i_code"',
       "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Subscribe"."State_NFC_Device"."History"[8]."ldt_ts"',
        "datatypeName":'DateTime'},
        ]


// for (i = 0; i < msg.payload.history.length; i++) {
// }


if (msg.payload.workpiece === null)
 {  msg.payload.workpiece = {};
    msg.payload.workpiece.id = "";
    msg.payload.workpiece.state = "";      
    msg.payload.workpiece.type = "";
 }

if (msg.payload.history === null)
 { msg.payload.history = []
   msg.payload.history[0] = {"code":0,"ts":""};
   msg.payload.history[1] = {"code":0,"ts":""};
   msg.payload.history[2] = {"code":0,"ts":""};
   msg.payload.history[3] = {"code":0,"ts":""};
   msg.payload.history[4] = {"code":0,"ts":""};
   msg.payload.history[5] = {"code":0,"ts":""};
   msg.payload.history[6] = {"code":0,"ts":""};
   msg.payload.history[7] = {"code":0,"ts":""};
   msg.payload.history[8] = {"code":0,"ts":""};
 }

if (msg.payload.history.length === 1)
 { msg.payload.history[1] = {"code":0,"ts":""};
   msg.payload.history[2] = {"code":0,"ts":""};
   msg.payload.history[3] = {"code":0,"ts":""};
   msg.payload.history[4] = {"code":0,"ts":""};
   msg.payload.history[5] = {"code":0,"ts":""};
   msg.payload.history[6] = {"code":0,"ts":""};
   msg.payload.history[7] = {"code":0,"ts":""};
   msg.payload.history[8] = {"code":0,"ts":""};
 }

if (msg.payload.history.length === 2)
 { msg.payload.history[2] = {"code":0,"ts":""};
   msg.payload.history[3] = {"code":0,"ts":""};
   msg.payload.history[4] = {"code":0,"ts":""};
   msg.payload.history[5] = {"code":0,"ts":""};
   msg.payload.history[6] = {"code":0,"ts":""};
   msg.payload.history[7] = {"code":0,"ts":""};
   msg.payload.history[8] = {"code":0,"ts":""};
 }

if (msg.payload.history.length === 3)
 { msg.payload.history[3] = {"code":0,"ts":""};
   msg.payload.history[4] = {"code":0,"ts":""};
   msg.payload.history[5] = {"code":0,"ts":""};
   msg.payload.history[6] = {"code":0,"ts":""};
   msg.payload.history[7] = {"code":0,"ts":""};
   msg.payload.history[8] = {"code":0,"ts":""};
 }

if (msg.payload.history.length === 4)
 { msg.payload.history[4] = {"code":0,"ts":""};
   msg.payload.history[5] = {"code":0,"ts":""};
   msg.payload.history[6] = {"code":0,"ts":""};
   msg.payload.history[7] = {"code":0,"ts":""};
   msg.payload.history[8] = {"code":0,"ts":""};
 }

if (msg.payload.history.length === 5)
 { msg.payload.history[5] = {"code":0,"ts":""};
   msg.payload.history[6] = {"code":0,"ts":""};
   msg.payload.history[7] = {"code":0,"ts":""};
   msg.payload.history[8] = {"code":0,"ts":""};
 }

if (msg.payload.history.length === 6)
 { msg.payload.history[6] = {"code":0,"ts":""};
   msg.payload.history[7] = {"code":0,"ts":""};
   msg.payload.history[8] = {"code":0,"ts":""};
 }

if (msg.payload.history.length === 7)
 { msg.payload.history[7] = {"code":0,"ts":""};
   msg.payload.history[8] = {"code":0,"ts":""};
 }

if (msg.payload.history.length === 8)
 { msg.payload.history[8] = {"code":0,"ts":""};
 }


msg.valuesToWrite = [
    msg.payload.ts,
    msg.payload.workpiece.id,
    msg.payload.workpiece.state,
    msg.payload.workpiece.type,
    msg.payload.history[0].code,
    msg.payload.history[0].ts,
    msg.payload.history[1].code,
    msg.payload.history[1].ts,
    msg.payload.history[2].code,
    msg.payload.history[2].ts,
    msg.payload.history[3].code,
    msg.payload.history[3].ts,
    msg.payload.history[4].code,
    msg.payload.history[4].ts,
    msg.payload.history[5].code,
    msg.payload.history[5].ts,
    msg.payload.history[6].code,
    msg.payload.history[6].ts,
    msg.payload.history[7].code,
    msg.payload.history[7].ts,
    msg.payload.history[8].code,
    msg.payload.history[8].ts,
    ]

msg.payload.hist_len = msg.payload.history.length;

return msg;
```

---

## 17. read NFC data from PLC

**Node ID**: `d4559cf5.61d818`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

//Items for NFC action
msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."s_cmd"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."Workpiece"."s_id"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."Workpiece"."s_state"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."Workpiece"."s_type"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[0]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[0]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[1]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[1]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[2]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[2]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[3]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[3]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[4]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[4]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[5]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[5]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[6]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[6]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[7]."i_code"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[7]."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[8]."i_code"',
       "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."History"[8]."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

return msg;
```

---

## 18. declare values

**Node ID**: `262cdf7d.826468`

### Codice JavaScript

```javascript
var ts = new Date().toISOString();

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Interface_TXT_Controler"."Publish"."ActionButtonNFCModule"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.payload["ts"] = ts;
msg.valuesToWrite = [ts];

return msg;
```

---

## 19. values to write

**Node ID**: `80286e18.ad8e5`

### Codice JavaScript

```javascript


msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."s_cmd"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."i_degree"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]


if (typeof msg.payload.degree === 'number')
 { msg.payload.degree = msg.payload.degree; }
else
 { msg.payload.degree = 0; }

msg.valuesToWrite = [
    msg.payload.cmd,
    msg.payload.degree,
    msg.payload.ts
    ]

return msg;
```

---

## 20. values to read

**Node ID**: `91483793.941ab8`

### Codice JavaScript

```javascript


msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."PosPanTiltUnit"."r_pan"',
        "datatypeName":'Float'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."PosPanTiltUnit"."r_tilt"',
        "datatypeName":'Float'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."PosPanTiltUnit"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]


return msg;
```

---

## 21. declare values

**Node ID**: `a66777a4.4a8f98`

### Codice JavaScript

```javascript
var ts = new Date().toISOString();

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."PosPanTiltUnit"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.payload["ts"] = ts;
msg.valuesToWrite = [ts];

return msg;
```

---

## 22. read data from PLC

**Node ID**: `1542a9f7.37e86e`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

//Items for NFC action
msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."s_location"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."ldt_ts"',
        "datatypeName":'DateTime'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[0,0]."s_id"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[0,0]."s_state"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[0,0]."s_type"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[0,1]."s_id"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[0,1]."s_state"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[0,1]."s_type"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[0,2]."s_id"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[0,2]."s_state"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[0,2]."s_type"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[1,0]."s_id"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[1,0]."s_state"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[1,0]."s_type"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[1,1]."s_id"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[1,1]."s_state"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[1,1]."s_type"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[1,2]."s_id"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[1,2]."s_state"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[1,2]."s_type"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[2,0]."s_id"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[2,0]."s_state"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[2,0]."s_type"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[2,1]."s_id"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[2,1]."s_state"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[2,1]."s_type"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[2,2]."s_id"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[2,2]."s_state"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."StockItem"[2,2]."s_type"',
        "datatypeName":'String'},
        ]

return msg;
```

---

## 23. declare values

**Node ID**: `7e81dbbd.876844`

### Codice JavaScript

```javascript
var ts = new Date().toISOString();

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."Stock_HBW"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.payload["ts"] = ts;
msg.valuesToWrite = [ts];

return msg;
```

---

## 24. values to write

**Node ID**: `14b7cf8f.2c3cf8`

### Codice JavaScript

```javascript


msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Publish"."OrderWorkpieceButton"."s_type"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Publish"."OrderWorkpieceButton"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.valuesToWrite = [
    msg.payload.type,
    msg.payload.ts
    ]

return msg;
```

---

## 25. read Order state

**Node ID**: `a54f69a6.6d0ec8`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

//Items for state Order
msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_Order"."s_state"',
        "datatypeName":'String'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_Order"."s_type"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_Order"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

return msg;
```

---

## 26. declare values

**Node ID**: `b557589f.7c33b`

### Codice JavaScript

```javascript
var ts = new Date().toISOString();

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."State_Order"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.payload["ts"] = ts;
msg.valuesToWrite = [ts];

return msg;
```

---

## 27. value to write

**Node ID**: `746e989a.9e1e48`

### Codice JavaScript

```javascript


msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."CameraPicture"."ldt_ts"',
        "datatypeName":'DateTime'},
//    {   "name":"",
//        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."CameraPicture"."s_data"',
//        "datatypeName":'String'},
        ]
msg.valuesToWrite = [
    msg.payload.ts,
//    msg.payload.data,
    ]

return msg;
```

---

## 28. values to write

**Node ID**: `3ac69873.ebbd68`

### Codice JavaScript

```javascript


msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."i_aq"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."di_gr"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_h"',
        "datatypeName":'Float'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."i_iaq"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_p"',
        "datatypeName":'Float'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_rh"',
        "datatypeName":'Float'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_rt"',
        "datatypeName":'Float'},
   {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."r_t"',
        "datatypeName":'Float'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."EnvironmentSensor"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.valuesToWrite = [
    msg.payload.aq,
    msg.payload.gr,
    msg.payload.h,
    msg.payload.iaq,
    msg.payload.p,
    msg.payload.rh,
    msg.payload.rt,
    msg.payload.t,
    msg.payload.ts,
]

return msg;
```

---

## 29. values to write

**Node ID**: `ecc1fb8d.138cd8`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
   {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."BrightnessSensor"."i_ldr"',
        "datatypeName":'Int16'},
   {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."BrightnessSensor"."r_br"',
        "datatypeName":'Float'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Interface_Dashboard"."Subscribe"."BrightnessSensor"."ldt_ts"',
        "datatypeName":'DateTime'},
        ]

msg.valuesToWrite = [
    msg.payload.ldr,
    msg.payload.br,
    msg.payload.ts,
]

return msg;
```

---

## 30. values to read

**Node ID**: `9c60c1c7.2c8f18`

### Codice JavaScript

```javascript


msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Horizontal_Axis"."di_Actual_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Horizontal_Axis"."di_Target_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Horizontal_Axis"."x_Position_Reached"',
        "datatypeName":'Boolean'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Vertical_Axis"."di_Actual_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Vertical_Axis"."di_Target_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Vertical_Axis"."x_Position_Reached"',
        "datatypeName":'Boolean'},
        ]

return msg;
```

---

## 31. declare values

**Node ID**: `64c49f07.3e14c8`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_HBW"."di_PosBelt_Horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 32. declare values

**Node ID**: `1cf34a00.49e556`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_HBW"."di_PosBelt_Vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 33. declare values

**Node ID**: `9175e6d9.13dd48`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_A1_Horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 34. declare values

**Node ID**: `f175b82e.fe3c58`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_A1_Vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 35. read config data from PLC

**Node ID**: `45f7dad0.91549c`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    //HBW
    //Pos. Belt
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosBelt_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosBelt_Vertical"',
        "datatypeName":'Int32'},
    //Offset Belt
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_Offset_Pos_Belt_Vertical"',
        "datatypeName":'Int32'},
    //Offset Rack
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_Offset_Pos_Rack_Vertical"',
        "datatypeName":'Int32'},
    //Pos. Rack A1
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_A1_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_A1_Vertical"',
        "datatypeName":'Int32'},
    //Pos. Rack B2
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_B2_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_B2_Vertical"',
        "datatypeName":'Int32'},
    //Pos. Rack C3
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_C3_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_C3_Vertical"',
        "datatypeName":'Int32'},
//VGR
//Pos Color
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_Color_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_Color_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_Color_rotate"',
        "datatypeName":'Int32'},
//Pos DSI
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_Collect_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_Discard_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_rotate"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_DSI_NFC_vertical"',
        "datatypeName":'Int32'},
//Pos DSO
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_Collect_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_Discard_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_rotate"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_DSO_vertical"',
        "datatypeName":'Int32'},
//Pos HBW
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_Collect_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_Discard_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_rotate"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_HBW_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_HBW_vertical"',
        "datatypeName":'Int32'},
//Pos MPO
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_MPO_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_MPO_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_MPO_rotate"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_MPO_vertical"',
        "datatypeName":'Int32'},
//Pos NFC
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NFC_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NFC_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NFC_rotate"',
        "datatypeName":'Int32'},
//Pos NiO
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NiO_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NiO_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NiO_rotate"',
        "datatypeName":'Int32'},
//Pos SLD Blue
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Blue_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Blue_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Blue_rotate"',
        "datatypeName":'Int32'},
//Pos SLD Red
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Red_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Red_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Red_rotate"',
        "datatypeName":'Int32'},
//Pos SLD White
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_White_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_White_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_White_rotate"',
        "datatypeName":'Int32'},
    //SSC    
    //Pos. Centre
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."di_Pos_Centre_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."di_Pos_Centre_Vertical"',
        "datatypeName":'Int32'},
    //Pos HBW
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."di_Pos_HBW_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."di_Pos_HBW_Vertical"',
        "datatypeName":'Int32'},

    //Color Sensor Calibration
    //DSI
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."w_Threshold_Red_Blue"',
        "datatypeName":'UInt16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."w_Threshold_White_Red"',
        "datatypeName":'UInt16'},

    //Color Sensor Calibration
    //SLD
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."w_Threshold_Red_Blue"',
        "datatypeName":'UInt16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."w_Threshold_White_Red"',
        "datatypeName":'UInt16'},
    //Pushout Counter
    //SLD
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."i_CounterValue_Blue"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."i_CounterValue_Red"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."i_CounterValue_White"',
        "datatypeName":'Int16'},
        ]

return msg;
```

---

## 36. declare values

**Node ID**: `72a051c6.5b7af`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
    //HBW
    //Pos. Belt
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosBelt_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosBelt_Vertical"',
        "datatypeName":'Int32'},
    //Offset Belt
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_Offset_Pos_Belt_Vertical"',
        "datatypeName":'Int32'},
    //Offset Rack
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_Offset_Pos_Rack_Vertical"',
        "datatypeName":'Int32'},
    //Pos. Rack A1
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_A1_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_A1_Vertical"',
        "datatypeName":'Int32'},
    //Pos. Rack B2
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_B2_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_B2_Vertical"',
        "datatypeName":'Int32'},
    //Pos. Rack C3
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_C3_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_C3_Vertical"',
        "datatypeName":'Int32'},
//VGR
//Pos Color
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_Color_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_Color_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_Color_rotate"',
        "datatypeName":'Int32'},
//Pos DSI
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_Collect_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_Discard_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_rotate"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_DSI_NFC_vertical"',
        "datatypeName":'Int32'},
//Pos DSO
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_Collect_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_Discard_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_rotate"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_DSO_vertical"',
        "datatypeName":'Int32'},
//Pos HBW
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_Collect_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_Discard_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_rotate"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_HBW_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_HBW_vertical"',
        "datatypeName":'Int32'},
//Pos MPO
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_MPO_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_MPO_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_MPO_rotate"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_MPO_vertical"',
        "datatypeName":'Int32'},
//Pos NFC
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NFC_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NFC_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NFC_rotate"',
        "datatypeName":'Int32'},
//Pos NiO
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NiO_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NiO_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NiO_rotate"',
        "datatypeName":'Int32'},
//Pos SLD Blue
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Blue_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Blue_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Blue_rotate"',
        "datatypeName":'Int32'},
//Pos SLD Red
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Red_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Red_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Red_rotate"',
        "datatypeName":'Int32'},
//Pos SLD White
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_White_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_White_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_White_rotate"',
        "datatypeName":'Int32'},
    //SSC    
    //Pos. Centre
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."di_Pos_Centre_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."di_Pos_Centre_Vertical"',
        "datatypeName":'Int32'},
    //Pos HBW
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."di_Pos_HBW_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."di_Pos_HBW_Vertical"',
        "datatypeName":'Int32'},

    //Color Sensor Calibration
    //DSI
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."w_Threshold_Red_Blue"',
        "datatypeName":'UInt16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."w_Threshold_White_Red"',
        "datatypeName":'UInt16'},

    //Color Sensor Calibration
    //SLD
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."w_Threshold_Red_Blue"',
        "datatypeName":'UInt16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."w_Threshold_White_Red"',
        "datatypeName":'UInt16'},
    //Pushout Counter
    //SLD
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."i_CounterValue_Blue"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."i_CounterValue_Red"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."i_CounterValue_White"',
        "datatypeName":'Int16'},
        ]


msg.valuesToWrite = [
                    //HBW
                    msg.payload.HBW_di_PosBelt_Horizontal,           //[0]
                    msg.payload.HBW_di_PosBelt_Vertical,             //[1]
                    msg.payload.HBW_di_PosBelt_Offset_Vertical,      //[2]

                    msg.payload.HBW_di_PosRack_Offset_Vertical,      //[3]
                    msg.payload.HBW_di_PosRack_A1_Horizontal,        //[4]
                    msg.payload.HBW_di_PosRack_A1_Vertical,          //[5]

                    msg.payload.HBW_di_PosRack_B2_Horizontal,        //[6]
                    msg.payload.HBW_di_PosRack_B2_Vertical,          //[7]

                    msg.payload.HBW_di_PosRack_C3_Horizontal,        //[8]
                    msg.payload.HBW_di_PosRack_C3_Vertical,          //[9]

                    //VGR
                    msg.payload.VGR_di_Pos_Color_horizontal,         //[10]
                    msg.payload.VGR_di_Pos_Color_vertical,           //[11]
                    msg.payload.VGR_di_Pos_Color_rotate,             //[12]
                    //Pos DSI
                    msg.payload.VGR_di_Pos_DSI_horizontal,           //[13]
                    msg.payload.VGR_di_Pos_DSI_Collect_vertical,     //[14]
                    msg.payload.VGR_di_Pos_DSI_Discard_vertical,     //[15]
                    msg.payload.VGR_di_Pos_DSI_rotate,               //[16]
                    msg.payload.VGR_di_Offset_Pos_DSI_NFC_vertical,  //[17]
                    //Pos DSO
                    msg.payload.VGR_di_Pos_DSO_horizontal,              //[18] 
                    msg.payload.VGR_di_Pos_DSO_Collect_vertical,        //[19]
                    msg.payload.VGR_di_Pos_DSO_Discard_vertical,        //[20] 
                    msg.payload.VGR_di_Pos_DSO_rotate,                  //[21] 
                    msg.payload.VGR_di_Offset_Pos_DSO_vertical,         //[22]
                    //Pos HBW
                    msg.payload.VGR_di_Pos_HBW_horizontal,              //[23] 
                    msg.payload.VGR_di_Pos_HBW_Collect_vertical,        //[24] 
                    msg.payload.VGR_di_Pos_HBW_Discard_vertical,        //[25] 
                    msg.payload.VGR_di_Pos_HBW_rotate,                  //[26] 
                    msg.payload.VGR_di_Offset_Pos_HBW_horizontal,       //[27] 
                    msg.payload.VGR_di_Offset_Pos_HBW_vertical,         //[28] 
                    //Pos MPO
                    msg.payload.VGR_di_Pos_MPO_horizontal,              //[29] 
                    msg.payload.VGR_di_Pos_MPO_vertical,                //[30] 
                    msg.payload.VGR_di_Pos_MPO_rotate,                  //[31] 
                    msg.payload.VGR_di_Offset_Pos_MPO_vertical,         //[32]
                    //Pos NFC
                    msg.payload.VGR_di_Pos_NFC_horizontal,              //[33] 
                    msg.payload.VGR_di_Pos_NFC_vertical,                //[34] 
                    msg.payload.VGR_di_Pos_NFC_rotate,                  //[35] 
                    //Pos NiO
                    msg.payload.VGR_di_Pos_NiO_horizontal,              //[36] 
                    msg.payload.VGR_di_Pos_NiO_vertical,                //[37] 
                    msg.payload.VGR_di_Pos_NiO_rotate,                  //[38] 
                    //Pos SLD Blue
                    msg.payload.VGR_di_Pos_SLD_Blue_horizontal,         //[39] 
                    msg.payload.VGR_di_Pos_SLD_Blue_vertical,           //[40] 
                    msg.payload.VGR_di_Pos_SLD_Blue_rotate,             //[41] 
                    //Pos SLD Red
                    msg.payload.VGR_di_Pos_SLD_Red_horizontal,          //[42] 
                    msg.payload.VGR_di_Pos_SLD_Red_vertical,            //[43] 
                    msg.payload.VGR_di_Pos_SLD_Red_rotate,              //[44] 
                    //Pos SLD White
                    msg.payload.VGR_di_Pos_SLD_White_horizontal,        //[45] 
                    msg.payload.VGR_di_Pos_SLD_White_vertical,          //[46] 
                    msg.payload.VGR_di_Pos_SLD_White_rotate,            //[47] 
                    //SSC
                    //Centre
                    msg.payload.SSC_di_PosCentre_Horizontal,         //[48]
                    msg.payload.SSC_di_PosCentre_Vertical,           //[49]
                    //HBW
                    msg.payload.SSC_di_PosHBW_Horizontal,            //[50]
                    msg.payload.SSC_di_PosHBW_Vertical,              //[51]

                    //Color Sensor Calibration
                    //DSI
                    msg.payload.SSC_w_Threshold_Red_Blue,            //[52]
                    msg.payload.SSC_w_Threshold_White_Red,           //[53]

                    //Color Sensor Calibration
                    //SLD
                    msg.payload.SLD_w_Threshold_Red_Blue,            //[54]
                    msg.payload.SLD_w_Threshold_White_Red,           //[55]
                    
                    //Pushout Counter
                    //SLD
                    msg.payload.SLD_i_CounterValue_Blue,         //[56]
                    msg.payload.SLD_i_CounterValue_Red,          //[57]
                    msg.payload.SLD_i_CounterValue_White,        //[58]

                    ];
return msg;



```

---

## 37. values to read

**Node ID**: `46fa3579.5e60ac`

### Codice JavaScript

```javascript


msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."horizontal_Axis"."di_Actual_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."vertical_Axis"."di_Actual_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."rotate_Axis"."di_Actual_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."horizontal_Axis"."di_Target_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."vertical_Axis"."di_Target_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."rotate_Axis"."di_Target_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."horizontal_Axis"."x_Position_Reached"',
        "datatypeName":'Boolean'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."vertical_Axis"."x_Position_Reached"',
        "datatypeName":'Boolean'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."rotate_Axis"."x_Position_Reached"',
        "datatypeName":'Boolean'},
        ]

return msg;
```

---

## 38. declare values

**Node ID**: `3f2b2126.0fe44e`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
            {
            "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_Color_horizontal"',
            "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 39. declare values

**Node ID**: `315e5775.d8984`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_Color_vertical"',
        "datatypeName":'Int32'
        },
                        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 40. declare values

**Node ID**: `102c3c27.3b9a14`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 41. declare values

**Node ID**: `d5bab92d.5f0f28`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 42. declare values

**Node ID**: `67e594bd.2e143c`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_Collect_vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 43. read config data from PLC

**Node ID**: `406252c4.ee950c`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

//Items for NFC action
msg.addressSpaceItems = [
//Pos Color
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_Color_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_Color_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_Color_rotate"',
        "datatypeName":'Int32'},
//Pos DSI
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_Collect_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_Discard_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_rotate"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_DSI_NFC_vertical"',
        "datatypeName":'Int32'},
//Pos DSO
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_Collect_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_Discard_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_rotate"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_DSO_vertical"',
        "datatypeName":'Int32'},
//Pos HBW
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_Collect_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_Discard_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_rotate"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_HBW_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_HBW_vertical"',
        "datatypeName":'Int32'},
//Pos MPO
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_MPO_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_MPO_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_MPO_rotate"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_MPO_vertical"',
        "datatypeName":'Int32'},
//Pos NFC
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NFC_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NFC_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NFC_rotate"',
        "datatypeName":'Int32'},
//Pos NiO
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NiO_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NiO_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NiO_rotate"',
        "datatypeName":'Int32'},
//Pos SLD Blue
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Blue_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Blue_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Blue_rotate"',
        "datatypeName":'Int32'},
//Pos SLD Red
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Red_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Red_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Red_rotate"',
        "datatypeName":'Int32'},
//Pos SLD White
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_White_horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_White_vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_White_rotate"',
        "datatypeName":'Int32'},
        ]

return msg;
```

---

## 44. declare values

**Node ID**: `95208776.418cf`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_Color_rotate"',
        "datatypeName":'Int32'
            
        },
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 45. declare values

**Node ID**: `6d87fa80.8dda4c`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_Collect_vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 46. declare values

**Node ID**: `3af1c81a.b27fe`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_rotate"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 47. declare values

**Node ID**: `41bb3419.82f8a4`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_DSI_NFC_vertical"',
        "datatypeName":'Int32'
            
        },
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 48. declare values

**Node ID**: `a93df711.ac90d8`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_rotate"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 49. declare values

**Node ID**: `732c5e17.29f5c8`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 50. declare values

**Node ID**: `c74204e0.a4f298`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_Collect_vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 51. declare values

**Node ID**: `bdcd2e34.7a61f8`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_rotate"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 52. declare values

**Node ID**: `b54423ee.50aee`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_HBW_horizontal"',
        "datatypeName":'Int32'
            
        },
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 53. declare values

**Node ID**: `b23796bd.423a88`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_HBW_vertical"',
        "datatypeName":'Int32'
            
        },
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 54. declare values

**Node ID**: `9596458e.46568`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_MPO_horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 55. declare values

**Node ID**: `11f1447d.75dc6c`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_MPO_vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 56. declare values

**Node ID**: `52b11f95.cb8da8`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_MPO_rotate"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 57. declare values

**Node ID**: `9895c6db.9b7cb`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NFC_horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 58. declare values

**Node ID**: `651e1072.cfbee8`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NFC_vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 59. declare values

**Node ID**: `553943c4.b70a7c`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NFC_rotate"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 60. read config data from PLC

**Node ID**: `b793ab10.b3a828`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    //HBW
    //Pos. Belt
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosBelt_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosBelt_Vertical"',
        "datatypeName":'Int32'},
    //Offset Belt
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_Offset_Pos_Belt_Vertical"',
        "datatypeName":'Int32'},
    //Offset Rack
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_Offset_Pos_Rack_Vertical"',
        "datatypeName":'Int32'},
    //Pos. Rack A1
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_A1_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_A1_Vertical"',
        "datatypeName":'Int32'},
    //Pos. Rack B2
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_B2_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_B2_Vertical"',
        "datatypeName":'Int32'},
    //Pos. Rack C3
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_C3_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_C3_Vertical"',
        "datatypeName":'Int32'},
        ]

return msg;
```

---

## 61. declare values

**Node ID**: `d0e7bcf5.1fff38`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSI_Discard_vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 62. declare values

**Node ID**: `77b4f933.922ed8`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_DSO_Discard_vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 63. declare values

**Node ID**: `db098de7.10304`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_DSO_vertical"',
        "datatypeName":'Int32'
            
        },
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 64. declare values

**Node ID**: `53b4b0e0.bf186`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_HBW_Discard_vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 65. declare values

**Node ID**: `44816851.c0cfc8`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Offset_Pos_MPO_vertical"',
        "datatypeName":'Int32'
            
        },
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 66. declare values

**Node ID**: `696757c8.d35fc`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NiO_horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 67. declare values

**Node ID**: `12d80646.3dce4a`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NiO_vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 68. declare values

**Node ID**: `a23013b4.ef0e28`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_NiO_rotate"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 69. declare values

**Node ID**: `d0603be8.c279a`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Blue_horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 70. declare values

**Node ID**: `f914c736.00a8e`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Blue_vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 71. declare values

**Node ID**: `f6736dd5.b63b8`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Blue_rotate"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 72. declare values

**Node ID**: `2401e3ee.a3373c`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Red_horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 73. declare values

**Node ID**: `7284e408.43618c`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Red_vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 74. declare values

**Node ID**: `ac9fb19b.cb4668`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_Red_rotate"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 75. declare values

**Node ID**: `6b88c6ee.e66b98`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_White_horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 76. declare values

**Node ID**: `9ca680b0.6f874`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_White_vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 77. declare values

**Node ID**: `6d76d171.e75098`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_VGR"."di_Pos_SLD_White_rotate"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 78. declare values

**Node ID**: `a42e8cbd.62f6b8`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_HBW"."di_Offset_Pos_Belt_Vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 79. declare values

**Node ID**: `573e4ed6.c8741`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_HBW"."di_Offset_Pos_Rack_Vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 80. declare values

**Node ID**: `b99c4fdc.99401`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_B2_Horizontal"',
        "datatypeName":'Int32'},





        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 81. declare values

**Node ID**: `23382617.7b103a`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
    
        {"nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_B2_Vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 82. declare values

**Node ID**: `cb9e76e4.6f9b08`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_C3_Horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 83. declare values

**Node ID**: `32338c0b.0ae58c`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_HBW"."di_PosRack_C3_Vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 84. values to read

**Node ID**: `4c600ea3.3cd76`

### Codice JavaScript

```javascript


msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."Horizontal_Axis"."di_Actual_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."Horizontal_Axis"."di_Target_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."Horizontal_Axis"."x_Position_Reached"',
        "datatypeName":'Boolean'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."Vertical_Axis"."di_Actual_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."Vertical_Axis"."di_Target_Position"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."Vertical_Axis"."x_Position_Reached"',
        "datatypeName":'Boolean'},
        ]

return msg;
```

---

## 85. declare values

**Node ID**: `684c44ac.f3b1b4`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_SSC"."di_Pos_Centre_Horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 86. declare values

**Node ID**: `100407.aab61bf9`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_SSC"."di_Pos_Centre_Vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 87. read config data from PLC

**Node ID**: `a9dbc99d.b4b588`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    //Pos. Centre
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."di_Pos_Centre_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."di_Pos_Centre_Vertical"',
        "datatypeName":'Int32'},
    //Pos HBW
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."di_Pos_HBW_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."di_Pos_HBW_Vertical"',
        "datatypeName":'Int32'},
        ]

return msg;
```

---

## 88. declare values

**Node ID**: `ca13ca6e.463098`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_SSC"."di_Pos_HBW_Horizontal"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 89. declare values

**Node ID**: `ebb0402c.ea9ee`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_SSC"."di_Pos_HBW_Vertical"',
        "datatypeName":'Int32'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 90. declare values

**Node ID**: `9d5e3e95.cdca7`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."i_Pos_Selection"',
        "datatypeName":'Int16'},
        ]

msg.valuesToWrite = [wert];

return msg;



```

---

## 91. declare values

**Node ID**: `4531d795.eebae`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Start_Positioning"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 92. declare values

**Node ID**: `291bd25c.8768fe`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Final_Positioning"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 93. values to read

**Node ID**: `2a981b73.7cdfec`

### Codice JavaScript

```javascript


msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."di_Pos_Horizontal"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."di_Pos_Vertical"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."di_Pos_Rotate"',
        "datatypeName":'Int32'},
        ]


return msg;
```

---

## 94. declare values

**Node ID**: `6b7ab88d.1ec6f8`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."i_Pos_Selection"',
        "datatypeName":'Int16'},
        ]

msg.valuesToWrite = [wert];

return msg;



```

---

## 95. declare values

**Node ID**: `99788681.3d7ef`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Start_Positioning"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 96. declare values

**Node ID**: `324f6e2c.b1624a`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Final_Positioning"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 97. declare values

**Node ID**: `ac8f7502.bd7648`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."i_Pos_Selection"',
        "datatypeName":'Int16'},
        ]

msg.valuesToWrite = [wert];

return msg;



```

---

## 98. declare values

**Node ID**: `6b343bc3.96af44`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Start_Positioning"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 99. declare values

**Node ID**: `a8f8a4d7.b4b1c`

### Codice JavaScript

```javascript
var wert = msg.payload


msg.nodetype = "inject";
msg.injectType = "write";



if (wert === true)
    {  
        msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Set_Pos_Activ"',
        "datatypeName":'Boolean'},
        ]
        msg.valuesToWrite = [wert];
    }
else
    {
        msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Set_Pos_Activ"',
        "datatypeName":'Boolean'},
        {"nodeId":'ns=3;s="gtyp_Setup"."i_Pos_Selection"',
        "datatypeName":'Int16'},
        ]
        msg.valuesToWrite = [wert,0];
    }

return msg;



```

---

## 100. declare values

**Node ID**: `36828d72.71cb62`

### Codice JavaScript

```javascript
var wert = msg.payload


msg.nodetype = "inject";
msg.injectType = "write";



if (wert === true)
    {  
        msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Set_Pos_Activ"',
        "datatypeName":'Boolean'},
        ]
        msg.valuesToWrite = [wert];
    }
else
    {
        msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Set_Pos_Activ"',
        "datatypeName":'Boolean'},
        {"nodeId":'ns=3;s="gtyp_Setup"."i_Pos_Selection"',
        "datatypeName":'Int16'},
        ]
        msg.valuesToWrite = [wert,0];
    }

return msg;



```

---

## 101. read config data from PLC

**Node ID**: `8a93ef88.964df`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [

    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."x_Set_Pos_Activ"',
        "datatypeName":'Boolean'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."i_Pos_Selection"',
        "datatypeName":'Int16'},

        ]

return msg;
```

---

## 102. declare values

**Node ID**: `2daabc9d.c95bcc`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Set_Calib_Value_Color_Blue"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 103. declare values

**Node ID**: `5216ed9e.0bbe6c`

### Codice JavaScript

```javascript
var wert = msg.payload


msg.nodetype = "inject";
msg.injectType = "write";

        msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Color_Sensor_Calibration"',
        "datatypeName":'Boolean'},
        ]
        msg.valuesToWrite = [wert];

return msg;



```

---

## 104. declare values

**Node ID**: `a27cc2e4.95e818`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Set_Calib_Value_Color_Red"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 105. declare values

**Node ID**: `2b705fc4.b6eae`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Set_Calib_Value_Color_White"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 106. declare values

**Node ID**: `297b82be.021c2e`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Home_Positioning"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 107. declare values

**Node ID**: `af48288e.999d58`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Start_Offset"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 108. declare values

**Node ID**: `c7401ab2.21b9e`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."i_Color_Sensor_Selection"',
        "datatypeName":'Int16'},
        ]

msg.valuesToWrite = [wert];

return msg;



```

---

## 109. values to read

**Node ID**: `32821c52.ea1854`

### Codice JavaScript

```javascript


msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."x_Color_Sensor_Calibration"',
        "datatypeName":'Boolean'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."i_Color_Sensor_Selection"',
        "datatypeName":'Int32'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."w_Calib_ColorValue_Blue"',
        "datatypeName":'Word'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."w_Calib_ColorValue_Red"',
        "datatypeName":'Word'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."w_Calib_ColorValue_White"',
        "datatypeName":'Word'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."w_Actual_ColorValue"',
        "datatypeName":'Word'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."w_Threshold_Red_Blue"',
        "datatypeName":'Word'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."w_Threshold_White_Red"',
        "datatypeName":'Word'},
    //Threshhold DSI
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."w_Threshold_Red_Blue"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SSC"."w_Threshold_White_Red"',
        "datatypeName":'Int16'},
    //Threshhold SLD
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."w_Threshold_Red_Blue"',
        "datatypeName":'Int16'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."w_Threshold_White_Red"',
        "datatypeName":'Int16'},
        ]
        

return msg;
```

---

## 110. declare values

**Node ID**: `1aa5f912.641677`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Clean_Rack_HBW"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 111. declare values

**Node ID**: `4ba1b27f.2078f4`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Home_Positioning"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 112. read config data from PLC

**Node ID**: `d2d7b546.3aac3`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    //Pos. Blue
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."x_Counter_Value_Calibration"',
        "datatypeName":'Boolean'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."i_Calib_CounterValue_Blue"',
        "datatypeName":'Int16'},
    //Pos. Red
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."i_Calib_CounterValue_Red"',
        "datatypeName":'Int16'},
    //Pos White
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."i_Calib_CounterValue_White"',
        "datatypeName":'Int16'},
    //act. Pos. Blue
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."i_CounterValue_Blue"',
        "datatypeName":'Int16'},
    //act. Pos. Red
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."i_CounterValue_Red"',
        "datatypeName":'Int16'},
    //act. Pos White
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."i_CounterValue_White"',
        "datatypeName":'Int16'},
        ]

return msg;
```

---

## 113. declare values

**Node ID**: `9b408ae5.691678`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."i_Calib_CounterValue_Blue"',
        "datatypeName":'Int16'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 114. declare values

**Node ID**: `f9742693.d0de88`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."i_Calib_CounterValue_Red"',
        "datatypeName":'Int16'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 115. declare values

**Node ID**: `ee0d9f41.9aa95`

### Codice JavaScript

```javascript
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."i_Calib_CounterValue_White"',
        "datatypeName":'Int16'},
        ]

msg.valuesToWrite = [wert];

return msg;
```

---

## 116. declare values

**Node ID**: `e22a46a0.13e348`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Set_CounterValues"',
        "datatypeName":'Boolean'},
        ]

msg.valuesToWrite = [wert];

return msg;



```

---

## 117. declare values

**Node ID**: `34181bef.baf07c`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Calculate_Value_Color"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 118. declare values

**Node ID**: `aabd31a.b36ef5`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_AcknowledgeButton"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 119. declare values

**Node ID**: `b4629dac.722198`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Home_Positioning"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 120. declare values

**Node ID**: `e2e8b027.48b018`

### Codice JavaScript

```javascript
var wert = msg.payload


msg.nodetype = "inject";
msg.injectType = "write";



if (wert === true)
    {  
        msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Set_Pos_Activ"',
        "datatypeName":'Boolean'},
        ]
        msg.valuesToWrite = [wert];
    }
else
    {
        msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Set_Pos_Activ"',
        "datatypeName":'Boolean'},
        {"nodeId":'ns=3;s="gtyp_Setup"."i_Pos_Selection"',
        "datatypeName":'Int16'},
        ]
        msg.valuesToWrite = [wert,0];
    }

return msg;



```

---

## 121. declare values

**Node ID**: `e85e0816.44758`

### Codice JavaScript

```javascript
var wert = msg.payload


msg.nodetype = "inject";
msg.injectType = "write";

        msg.addressSpaceItems = [
        {
        "nodeId":'ns=3;s="gtyp_Setup"."x_Counter_Value_Calibration"',
        "datatypeName":'Boolean'
        },
        ]
        msg.valuesToWrite = [wert];

return msg;



```

---

## 122. values to read

**Node ID**: `d46bbb03.35203`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    //ROW A
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[0,0]."s_id"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[0,0]."s_state"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[0,0]."s_type"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[0,1]."s_id"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[0,1]."s_state"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[0,1]."s_type"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[0,2]."s_id"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[0,2]."s_state"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[0,2]."s_type"',
        "datatypeName":'STRING'},
    //ROW B
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[1,0]."s_id"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[1,0]."s_state"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[1,0]."s_type"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[1,1]."s_id"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[1,1]."s_state"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[1,1]."s_type"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[1,2]."s_id"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[1,2]."s_state"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[1,2]."s_type"',
        "datatypeName":'STRING'},
    //ROW C
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[2,0]."s_id"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[2,0]."s_state"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[2,0]."s_type"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[2,1]."s_id"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[2,1]."s_state"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[2,1]."s_type"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[2,2]."s_id"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[2,2]."s_state"',
        "datatypeName":'STRING'},
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_HBW"."Rack_Workpiece"[2,2]."s_type"',
        "datatypeName":'STRING'},
        ]

return msg;
```

---

## 123. declare values

**Node ID**: `a9cdc024.a4fdf`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Park_Position"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 124. read config data from PLC

**Node ID**: `702d88ad.9399e`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    //act. Pos. Blue
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."i_CounterValue_Blue"',
        "datatypeName":'Int16'},
    //act. Pos. Red
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."i_CounterValue_Red"',
        "datatypeName":'Int16'},
    //act. Pos White
    {   "name":"",
        "nodeId":'ns=3;s="gtyp_SLD"."i_CounterValue_White"',
        "datatypeName":'Int16'},
        ]

return msg;
```

---

## 125. declare values

**Node ID**: `6f29f312.42b8bc`

### Codice JavaScript

```javascript
//var ts = new Date().toISOString();
var wert = msg.payload

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
        {"nodeId":'ns=3;s="gtyp_Setup"."x_Fill_Rack_HBW"',
        "datatypeName":'Boolean'},
        ]

//msg.payload["ts"] = ts;
msg.valuesToWrite = [wert];

return msg;



```

---

## 126. values to read

**Node ID**: `f65e8e34.314df`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "read";

msg.addressSpaceItems = [
    //act. Pos. Blue
    {
        "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."x_Start_TON_Fill_HBW"',
        "datatypeName":'Boolean'},
    {
        "name":"",
        "nodeId":'ns=3;s="gtyp_Setup"."r_Version_SPS"',
        "datatypeName":'Float'},

        ]

return msg;
```

---

## 127. prepare HBW order

**Node ID**: `7af27e81.3fe4b`

### Codice JavaScript

```javascript
var ts = new Date().toISOString();

if (!msg.payload || !msg.payload.type) {
    node.warn("Payload non valido: atteso { type: 'BLUE|RED|WHITE' }");
    return null;
}

msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
  {
    "nodeId": "ns=3;s=\"gtyp_Interface_Dashboard\".\"Publish\".\"OrderWorkpieceButton\".\"s_type\"",
    "datatypeName": "String"
  },
  {
    "nodeId": "ns=3;s=\"gtyp_Interface_Dashboard\".\"Publish\".\"OrderWorkpieceButton\".\"ldt_ts\"",
    "datatypeName": "DateTime"
  }
];

msg.valuesToWrite = [
  msg.payload.type,
  ts
];

return msg;
```

---

## 128. Select SSC Position

**Node ID**: `1bee293e.bf528f`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "write";

// 0 = CENTER, 1 = HBW
let posValue = (msg.preset === "CENTER") ? 0 : 1;

msg.addressSpaceItems = [
  {
    "nodeId": "ns=3;s=\"gtyp_Interface_Dashboard\".\"SSC_Positions\"",
    "datatypeName": "Int16"
  }
];

msg.valuesToWrite = [posValue];

return msg;
```

---

## 129. Start Positioning

**Node ID**: `f372538f.48f9a`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
  {
    "nodeId": "ns=3;s=\"gtyp_Setup\".\"x_Start_Positioning\"",
    "datatypeName": "Boolean"
  }
];

msg.valuesToWrite = [true];

return msg;
```

---

## 130. write PTU setpoints

**Node ID**: `f7dafce8.ada9`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "write";

msg.addressSpaceItems = [
  {
    nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."w_pan"',
    datatypeName: 'Float',
    value: msg.pan
  },
  {
    nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."w_tilt"',
    datatypeName: 'Float',
    value: msg.tilt
  }
];

return msg;
```

### NodeId trovati in questo function:

- `ns=3;s=`
- `ns=3;s=`

---

## 131. start positioning

**Node ID**: `e5c1bb4b.766a28`

### Codice JavaScript

```javascript
msg.nodetype = "inject";
msg.injectType = "write";
msg.addressSpaceItems = [
  {
    nodeId: 'ns=3;s="gtyp_Interface_Dashboard"."Publish"."PosPanTiltUnit"."cmd_start"',
    datatypeName: 'Boolean',
    value: true
  }
];
return msg;
```

### NodeId trovati in questo function:

- `ns=3;s=`

---

