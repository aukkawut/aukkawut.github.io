---
layout: summary
---

# DALL-E 2

*This summary is summarizing and explaining DALL-E 2 [(https://arxiv.org/abs/2204.06125)](https://arxiv.org/abs/2204.06125) paper in Thai.* Underconstruction

## สรุปแบบย่อ

DALL-E 2 เป็น Encoder-Decoder Model ที่แบ่งออกเป็นสองส่วน ส่วนแรกเป็น Encoder ใช้ CLIP เพื่อแปลงข้อความให้เป็นภาพผ่านการ encode ข้อความให้เป็น embedding แล้วแปลงให้เป็น embedding ภาพเพื่อสร้าง prior distribution ในการสร้างภาพ และส่วนที่สองคือ Decoder ทำการสร้างภาพจาก embedding บน prior distribution

<img width="916" alt="fig2" src="https://user-images.githubusercontent.com/50354662/212390595-3cdae2aa-9601-4927-9697-bd945f7b8754.png" style="display:block; margin-left:auto; margin-right:auto" >

ข้อดีของการทำอย่างนี้ก็คือเราสามารถสร้างภาพที่มีความแตกต่างกันได้จากข้อความอันเดียว รวมไปถึงสามารถทำ Zero-shot Learning เพื่อสร้างภาพที่ไม่เคยมีอยู่มาก่อนจากข้อความที่ไม่เคยเห็นมาก่อนได้

## บทนำ : Latent Variable

ลองนึกถึงเวลาที่เรานั่งทำงานแล้วเห็นเพื่อนร่วมงานตัวเปียก แน่นอนว่าเราสามารถมองภาพได้ว่าไม่นานมานี้ ข้างนอกฝนตกอยู่ โดยที่เราไม่ต้องหันไปมองหน้าต่างข้างนอกว่าฝนตกหรือไม่

![image](https://user-images.githubusercontent.com/50354662/212489998-b8abfee4-f9f6-49d0-88c7-7033feab7ba7.png){:style="display:block; margin-left:auto; margin-right:auto"}

เราเรียกข้อมูลที่เราไม่รู้ตรงๆ (เช่น ตอนนี้ฝนตกหรือไม่) ที่สามารถบอกได้ว่าข้างนอกฝนตกหรือไม่ (เช่่น ตัวเปียก) ว่า Latent variables และเจ้านี้เป็นเหมือนกับตัวแทน หรือ representation ของสภาพอากาศข้างนอกได้

## นอกเรื่องไป Meta Learning

### บทนำสู่ Meta Learning: Distance-based Learning
สมมติผมอยากสอน Neural network ให้จำแนกระหว่างเลขที่เขียนด้วยมือสองตัว 0 กับเลข 1 เช่น (1,0) หรือ (0,0) ว่าเลขทั้งสองตัวเป็นเลขเดียวกันไหม ตามปกติเราก็สามารถสร้าง Neural networks ที่จำแนกว่าเลขแรกเป็นเลขอะไร และเลขที่สองเป็นเลขอะไรแล้วมาเทียบกัน กล่าวคือเราทำการ Train NN บน cross-entropy loss (จากการที่ Response/output ของเราเป็น Binary output เหมือนใน Bernoulli process)

แต่เราจะทำอย่างนั้นทำไมในเมื่อเราสามารถ "สอนให้ NN เรียนรู้" การแยกความแตกต่างได้ อ่ะ เราลองทำแบบแรกก่อนแล้วดูว่า NN มันมองตัวเลขเป็นอย่างไง

{% include_relative latent.html max-width="300px" %}

ตาม t-SNE plot ของเวกเตอร์ใน layer สุดท้ายของ NN แบบปกติของเรา เลข 0 จะแทนด้วยสีแดงและเลข 1 จะแทนด้วยสีฟ้า จะเห็นว่าเลขกลุ่มเดียวกันจะอยู่ใกล้กันบน latent space! ทำให้เราสามารถนิยาม euclidean metric หรืออาจจะเป็น cosine similarity, inner product, etc. เพื่อเทียบว่าตัวเลขดังกล่าวเป็นเลขเดียวกันหรือไม่

### แล้วจะทำอย่างนี้ไปเพื่ออะไร

![dang](https://user-images.githubusercontent.com/50354662/212450227-2914514f-beb2-4041-a7be-91faa2cc9858.jpg){:style="display:block; margin-left:auto; margin-right:auto"}


สมมติว่าเรามีตัวเลขทั้งหมด 10 ตัว หรือถ้าเรามีภาพชิ้นเนื้อแล้วเราอยากรู้ว่าคนนั้นบาดเจ็บอย่างไง แน่นอนการสร้าง Model ที่จำแนกทุกอย่างได้ดีกว่า แต่เราก็สามารถใช้วิธีง่ายๆ นี้ในการทำให้ชีวิตของเราง่ายขึ้นได้

### แล้วเกี่ยวอะไรกับงานนี้

Spoiler Alert: Zero-shot learning ใช้แนวคิดคล้ายๆ กันกับตัวอย่างก่อนหน้าที่เราอธิบายไป

คือจากตัวอย่างก่อนหน้า สมมติเราใช้ Cosine similarity จะเห็นว่าเราสามารถนำ Embedding มาเทียบเพื่อหาว่าภาพแรกต่างจากภาพสองหรือไม่ แต่เราไม่จำเป็นต้องหาความแตกต่างอย่างเดียว แต่เราสามารถใช้การที่เราสามารถเทียบค่าเหล่านั้นได้เพื่อเป็นการเข้าใจในข้อความ และเข้าใจตัวรูปภาพได้ มาดูตัวอย่างต่อไปกัน

![manifold](https://user-images.githubusercontent.com/50354662/212417602-5cc0e99a-2c41-40c0-84ae-dd175d872943.svg){:style="display:block; margin-left:auto; margin-right:auto;max-width: 300px;"}

ภาพนี้สร้างจาก Decoder ที่รับ input เป็นเวกเตอร์สองมิติ (ค่าแสดงโดยแกนมิติต่างๆ) ของ Variational Autoencoder (VAE model คล้ายๆ กับ DALL-E 2) ที่ถูกสอนด้วย MNIST data จะเห็นว่าการที่เราเคลื่อนที่ไปตามแกนต่างๆ (ความกลม ความรี หรืออะไรก็ว่าไปที่เป็น Latent variables) ก็จะส่งผลให้ภาพที่สร้างขึ้นต่างกัน แต่สิ่งหนึ่งที่น่าสนใจจากตรงนี้คือ ตัว NN ไม่จำเป็นต้องเคยเห็นค่าเหล่านั้นมาก่อน ก็สามารถสร้างภาพเหล่านั้นได้ และคล้ายกับตัวอย่างก่อนหน้า ค่าที่ใกล้ๆ กัน (ในแต่ละมิติ) ก็จะให้ภาพที่ีคล้ายคลึงกัน 

ทีนี้ลองมองภาพให้ใหญ่ขึ้น สมมติผมต้องการจำแนกภาพด้วย model ที่ผม train จาก dataset หนึ่ง (เรียก $\mathcal{D}_0$) ผมมี label ที่ผมอยากให้มันเรียน และ label ที่ผมใช้ train ตอนแรก แต่ผมไม่มีภาพใหม่ให้มัน train (zero data -> zero shot) เราจะทำอย่างไงให้มันเรียนรู้ที่จะจำแนกภาพใหม่ๆ ที่มันไม่เคยเจอ ด้วย label อันไหม่ได้?

<img width="1316" alt="Screenshot 2023-01-14 at 1 35 54 PM" src="https://user-images.githubusercontent.com/50354662/212490336-06be64c2-2162-4fd8-b560-b2c4a6c66e8d.png" style="display:block; margin-left:auto; margin-right:auto">

ในตัวอย่างนี้ สมมติว่าเรามีภาพ Liger แต่เราไม่เคยสอนให้มันรู้จัก Liger มาก่อน จากตัวอย่างก่อนหน้า สิ่งที่เราทำได้คือเทียบว่า Liger มันเหมือนกับคำว่าอะไรบ้าง

## อะไรคือ Diffusion Model





## แล้ว CLIP คืออะไร

## แล้วมันเกี่ยวอะไรกับ Paper นี้
