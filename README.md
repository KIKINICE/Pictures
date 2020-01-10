# 毕业纪念相册
  ### PRD 价值主张设计
  #### PRD1.加值宣言

* 毕业纪念册设计制作的意义在于，一本毕业纪念册它是记录着学生们在校园成长、生活的历程，是小到幼儿园，大到大学、研究生，甚至是博士毕业，中间还有小学、初中、高中年级，所有这些学年的同学们，在校园的学习与成长的记忆，汇聚成一本完整的回忆，回忆上面是照片与文字的组合，是我们的学生、同学们在校园内、在校园外的各种学习、活动照片，以及各种抒情文字、寄语内容，来解释当时的画面、发生的事情与当时的心情等。
* 也许，我们再毕业之初，并不懂得毕业纪念册设计有什么作用，只是觉得好看或者想念时可以翻翻看看，可能会觉得是可有可无的东西。这是因为，我们的阅历尚浅。待到步入社会、步入中年之后，当一些温饱问题都得到解决之后，人脉、社交、同学、校友的情感等问题，则会一一迎面而来。这时候，我们再回头看看，也许就会想到当初为何不做一本毕业纪念册，记录下当年，还青春年少，也许还青涩的我们，以至于留下终生遗憾。当人步入老年之后，平时闲来无事，想要回忆往昔清楚年少时的我们，却已经找不到回忆的那些片段、线索了。而一本毕业纪念册的存在，就是这些回忆、片段的载体，我们手捧着一本毕业纪念册的时候，每一个画面，历历在目。
#### PRD2.核心价值宣言
随着微商的猖獗，现代人越来越不喜欢发朋友圈，但是还是会有聚会留下的照片，照片在手机又会占据内存。所以一款非常注意隐私的毕业纪念相册横跨出世。该纪念相册可以留下通过人脸识别API判断年龄，为照片增加趣味性。另外相册需要创建相册的人邀请才能观看相册，非常注重隐私性。

 #### PRD3.用户痛点 3%
  用户痛点宣言：
* 想要把美好的回忆都保存下来，但是又不想发朋友圈发微博，被观摩，只想有在相册里的人看。
* 相册要有留言功能，每次回来回忆时，用户新填新的留言
* 虽然相片有时间记录，但是没有app能帮忙做出时间轴。

#### PRD4.人工智能概率性
AI概率性考量：
* api无法调用的风险，key使用次数的限制，无法使用产品api
* api出现错误的风险
* 用户上传错误数据，导致api分析结果不显示的风险。
#### PRD5.需求列表
* 需求列表：

| 用户需求 | 纪念意义         | 趣味性   |
| -------- | ---------------- | -------- |
|          | 相册保存照片功能 | 人脸识别 |

 #### 原型4口头操作说明 

口头操作说明：
一位用户上传照片创建相册，相当于为册主，册主邀请用户加入相册，其他用户与册主一样拥有上传、删除、浏览相册的权限。但是册主拥有相册用户的 决定权，只有册主可以邀请、移除用户。用户在相册时，可以看到相片通过api计算出来的年龄，以及在干得事情，让相册富有趣味性。
 #### API1.使用水平 5%
* Azure人脸识别API输入：
```
 -*- coding: utf-8 -*-
import urllib.request
import urllib.error
import time

http_url = '[https://api-cn.faceplusplus.com/facepp/v3/detect](https://api-cn.faceplusplus.com/facepp/v3/detect)'
key = "jZcg7VD0avxBw4dtyZGfV2a18ALqjWkA"
secret = "0Wih6rArIYUyR6BvliKS4ImvW8mTWmfy"
filepath = r"C:\Users\DELL\Desktop\timg.jpg"

boundary = '----------%s' % hex(int(time.time() * 1000))
data = []
data.append('--%s' % boundary)
data.append('Content-Disposition: form-data; name="%s"\r\n' % 'api_key')
data.append(key)
data.append('--%s' % boundary)
data.append('Content-Disposition: form-data; name="%s"\r\n' % 'api_secret')
data.append(secret)
data.append('--%s' % boundary)
fr = open(filepath, 'rb')
data.append('Content-Disposition: form-data; name="%s"; filename=" "' % 'image_file')
data.append('Content-Type: %s\r\n' % 'application/octet-stream')
data.append(fr.read())
fr.close()
data.append('--%s' % boundary)
data.append('Content-Disposition: form-data; name="%s"\r\n' % 'return_landmark')
data.append('1')
data.append('--%s' % boundary)
data.append('Content-Disposition: form-data; name="%s"\r\n' % 'return_attributes')
data.append(
    "gender,age,smiling,headpose,facequality,blur,eyestatus,emotion,ethnicity,beauty,mouthstatus,eyegaze,skinstatus")
data.append('--%s--\r\n' % boundary)

for i, d in enumerate(data):
    if isinstance(d, str):
        data[i] = d.encode('utf-8')

http_body = b'\r\n'.join(data)

# build http request
req = urllib.request.Request(url=http_url, data=http_body)

# header
req.add_header('Content-Type', 'multipart/form-data; boundary=%s' % boundary)

try:
    # post data to server
    resp = urllib.request.urlopen(req, timeout=5)
    # get response
    qrcont = resp.read()
    # if you want to load as json, you should decode first,
    # for example: json.loads(qrount.decode('utf-8'))
    print(qrcont.decode('utf-8'))
except urllib.error.HTTPError as e:
    print(e.read().decode('utf-8'))
```
* API的输出
```
{"time_used": 368, "faces": [{"landmark": {"mouth_upper_lip_left_contour2": {"y": 391, "x": 435}, "mouth_upper_lip_top": {"y": 392, "x": 457}, "mouth_upper_lip_left_contour1": {"y": 389, "x": 448}, "left_eye_upper_left_quarter": {"y": 283, "x": 400}, "left_eyebrow_lower_middle": {"y": 266, "x": 405}, "mouth_upper_lip_left_contour3": {"y": 398, "x": 440}, "right_eye_top": {"y": 282, "x": 515}, "left_eye_bottom": {"y": 301, "x": 410}, "right_eyebrow_lower_left_quarter": {"y": 271, "x": 505}, "right_eye_pupil": {"y": 291, "x": 513}, "mouth_lower_lip_right_contour1": {"y": 400, "x": 477}, "mouth_lower_lip_right_contour3": {"y": 415, "x": 475}, "mouth_lower_lip_right_contour2": {"y": 406, "x": 487}, "contour_chin": {"y": 451, "x": 461}, "contour_left9": {"y": 445, "x": 436}, "left_eye_lower_right_quarter": {"y": 300, "x": 423}, "mouth_lower_lip_top": {"y": 402, "x": 458}, "right_eyebrow_upper_middle": {"y": 255, "x": 522}, "left_eyebrow_left_corner": {"y": 265, "x": 366}, "right_eye_bottom": {"y": 302, "x": 517}, "contour_left7": {"y": 413, "x": 400}, "contour_left6": {"y": 396, "x": 386}, "contour_left5": {"y": 376, "x": 375}, "contour_left4": {"y": 355, "x": 367}, "contour_left3": {"y": 333, "x": 361}, "contour_left2": {"y": 311, "x": 357}, "contour_left1": {"y": 288, "x": 355}, "left_eye_lower_left_quarter": {"y": 297, "x": 398}, "contour_right1": {"y": 292, "x": 566}, "contour_right3": {"y": 336, "x": 558}, "contour_right2": {"y": 314, "x": 563}, "mouth_left_corner": {"y": 392, "x": 422}, "contour_right4": {"y": 357, "x": 551}, "contour_right7": {"y": 415, "x": 519}, "right_eyebrow_left_corner": {"y": 272, "x": 485}, "nose_right": {"y": 360, "x": 488}, "nose_tip": {"y": 358, "x": 462}, "contour_right5": {"y": 377, "x": 543}, "nose_contour_lower_middle": {"y": 372, "x": 462}, "left_eyebrow_lower_left_quarter": {"y": 264, "x": 386}, "mouth_lower_lip_left_contour3": {"y": 414, "x": 441}, "right_eye_right_corner": {"y": 291, "x": 536}, "right_eye_lower_right_quarter": {"y": 298, "x": 528}, "mouth_upper_lip_right_contour2": {"y": 392, "x": 481}, "right_eyebrow_lower_right_quarter": {"y": 265, "x": 542}, "left_eye_left_corner": {"y": 290, "x": 391}, "mouth_right_corner": {"y": 394, "x": 497}, "mouth_upper_lip_right_contour3": {"y": 399, "x": 476}, "right_eye_lower_left_quarter": {"y": 301, "x": 504}, "left_eyebrow_right_corner": {"y": 272, "x": 445}, "left_eyebrow_lower_right_quarter": {"y": 271, "x": 425}, "right_eye_center": {"y": 293, "x": 515}, "nose_left": {"y": 359, "x": 434}, "mouth_lower_lip_left_contour1": {"y": 399, "x": 440}, "left_eye_upper_right_quarter": {"y": 287, "x": 426}, "right_eyebrow_lower_middle": {"y": 267, "x": 524}, "left_eye_top": {"y": 281, "x": 413}, "left_eye_center": {"y": 293, "x": 412}, "contour_left8": {"y": 430, "x": 416}, "contour_right9": {"y": 445, "x": 486}, "right_eye_left_corner": {"y": 298, "x": 491}, "mouth_lower_lip_bottom": {"y": 419, "x": 458}, "left_eyebrow_upper_left_quarter": {"y": 254, "x": 385}, "left_eye_pupil": {"y": 290, "x": 411}, "right_eyebrow_upper_left_quarter": {"y": 260, "x": 502}, "contour_right8": {"y": 431, "x": 504}, "right_eyebrow_right_corner": {"y": 265, "x": 560}, "right_eye_upper_left_quarter": {"y": 287, "x": 501}, "left_eyebrow_upper_middle": {"y": 256, "x": 407}, "right_eyebrow_upper_right_quarter": {"y": 254, "x": 543}, "nose_contour_left1": {"y": 297, "x": 447}, "nose_contour_left2": {"y": 342, "x": 440}, "mouth_upper_lip_right_contour1": {"y": 390, "x": 467}, "nose_contour_right1": {"y": 298, "x": 476}, "nose_contour_right2": {"y": 343, "x": 482}, "mouth_lower_lip_left_contour2": {"y": 404, "x": 431}, "contour_right6": {"y": 397, "x": 532}, "nose_contour_right3": {"y": 367, "x": 476}, "nose_contour_left3": {"y": 367, "x": 447}, "left_eye_right_corner": {"y": 298, "x": 435}, "left_eyebrow_upper_right_quarter": {"y": 261, "x": 427}, "right_eye_upper_right_quarter": {"y": 284, "x": 527}, "mouth_upper_lip_bottom": {"y": 401, "x": 457}}, "attributes": {"emotion": {"sadness": 0.025, "neutral": 82.256, "disgust": 0.025, "anger": 0.041, "surprise": 10.904, "fear": 0.025, "happiness": 6.725}, "beauty": {"female_score": 83.064, "male_score": 85.913}, "gender": {"value": "Female"}, "age": {"value": 22}, "mouthstatus": {"close": 99.951, "surgical_mask_or_respirator": 0.0, "open": 0.049, "other_occlusion": 0.0}, "glass": {"value": "None"}, "skinstatus": {"dark_circle": 5.435, "stain": 9.526, "acne": 4.507, "health": 86.207}, "headpose": {"yaw_angle": 1.4644147, "pitch_angle": 11.593095, "roll_angle": 3.9076579}, "blur": {"blurness": {"threshold": 50.0, "value": 0.337}, "motionblur": {"threshold": 50.0, "value": 0.337}, "gaussianblur": {"threshold": 50.0, "value": 0.337}}, "smile": {"threshold": 50.0, "value": 1.7}, "eyestatus": {"left_eye_status": {"normal_glass_eye_open": 0.044, "no_glass_eye_close": 0.0, "occlusion": 0.0, "no_glass_eye_open": 99.956, "normal_glass_eye_close": 0.0, "dark_glasses": 0.0}, "right_eye_status": {"normal_glass_eye_open": 0.038, "no_glass_eye_close": 0.0, "occlusion": 0.0, "no_glass_eye_open": 99.961, "normal_glass_eye_close": 0.0, "dark_glasses": 0.001}}, "facequality": {"threshold": 70.1, "value": 90.037}, "ethnicity": {"value": "ASIAN"}, "eyegaze": {"right_eye_gaze": {"position_x_coordinate": 0.453, "vector_z_component": 0.979, "vector_x_component": -0.135, "vector_y_component": -0.151, "position_y_coordinate": 0.426}, "left_eye_gaze": {"position_x_coordinate": 0.492, "vector_z_component": 0.979, "vector_x_component": 0.147, "vector_y_component": -0.138, "position_y_coordinate": 0.414}}}, "face_rectangle": {"width": 211, "top": 246, "left": 354, "height": 211}, "face_token": "0c744faa710e30f61b2e1024cb14e4c7"}], "image_id": "WFlS739/QDNmXqJ44j52fw==", "request_id": "1568646710,22551e7c-f0eb-452c-b7f3-3f7ad24c7ab6", "face_num": 1}
```
#### API2.使用比较分析 5%

 使用比较分析：在PRD文件中是否有说明且提供连结证据，所使用的API是查找过最适用的（主要竞争者无或比较次），如考量其成熟度丶性价比丶等等

#### API3.使用后风险报告 5%
* api无法调用的风险，key使用次数的限制，无法使用产品api
* api出现错误的风险
* 用户上传错误数据，导致api分析结果不显示的风险。
