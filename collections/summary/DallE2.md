---
layout: summary
---

# DALL-E 2

*This summary is summarizing DALL-E 2 paper in Thai.* Underconstruction

## สรุปแบบย่อ

DALL-E 2 เป็น Model ที่ใช้ CLIP เพื่อแปลงข้อความให้เป็นภาพผ่านการ encode  
<img width="916" alt="fig2" src="https://user-images.githubusercontent.com/50354662/212390595-3cdae2aa-9601-4927-9697-bd945f7b8754.png">

## บทนำ

เนื่องด้วยช่วงนี้งานวิจัยทางด้าน Computer Vision 

## อะไรคือ Diffusion Model

## นอกเรื่องไป Meta Learning

สมมติผมอยากสอน Neural network ให้จำแนกระหว่างเลขที่เขียนด้วยมือสองตัว 0 กับเลข 1 เช่น (1,0) หรือ (0,0) ว่าเลขทั้งสองตัวเป็นเลขเดียวกันไหม ตามปกติเราก็สามารถสร้าง Neural networks ที่จำแนกว่าเลขแรกเป็นเลขอะไร และเลขที่สองเป็นเลขอะไรแล้วมาเทียบกัน กล่าวคือเราทำการ Train NN บน cross-entropy loss (จากการที่ Response/output ของเราเป็น Binary output เหมือนใน Bernoulli process)

แต่เราจะทำอย่างนั้นทำไมในเมื่อเราสามารถ "สอนให้ NN เรียนรู้" การแยกความแตกต่างได้ อ่ะ เราลองทำแบบแรกก่อนแล้วดูว่า NN มันมองตัวเลขเป็นอย่างไง
{% include_relative latent.html %}

## แล้ว CLIP คืออะไร

## แล้วมันเกี่ยวอะไรกับ Paper นี้
