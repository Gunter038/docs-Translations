- - -
การแปลเอกสาร
- - -

# การสนับสนุนการแปลโดยชุมชน

หากคุณเป็นสมาชิกชุมชน Celestia ที่มีความกระตือรือร้นและต้องการมีส่วนร่วมในการแปลหน้าเอกสาร นี่คือคำแนะนำสำหรับคุณ

## เยี่ยมชมโครงการ Crowdin ของเรา

ในการเริ่มต้น ไปที่โครงการ Crowdin ที่นี่

คุณจะต้องสร้างบัญชี จากนั้นคุณจะสามารถเข้าร่วมโครงการเพื่อเริ่มต้นการแปลของคุณ

หากคุณไม่เห็นภาษาของคุณ โปรดสอบถามในช่อง #translations บน Discord ที่นี่

ใน Crowdin คุณสามารถแปล แสดงความคิดเห็นเกี่ยวกับการแปล และยังให้การโหวตเห็นด้วยและไม่เห็นด้วยในการแปลที่มีอยู่

แสดงความคิดเห็นของคุณเกี่ยวกับการแปลที่มีอยู่เพื่อให้แน่ใจว่าถูกต้อง!

## เคล็ดลับ

ต่อไปนี้คือเคล็ดลับเล็กๆ น้อยๆ ที่จะช่วยคุณในระหว่างการแปล

### เอกสาร Crowdin

เอกสารประกอบอย่างเป็นทางการของ Crowdin

### คำแนะนำ

#### โค้ด

บางหน้ามีข้อมูลและรหัสคอมพิวเตอร์

It is important to keep in mind that William Shakespeare was an English speaker...So was Alan Turing! That is why you should not translate parts of the code "itself".

For instance, if you see metadata like `sidebar_label : Hello World`, a French translation would be `sidebar_label : Salut tout le monde`.

Let's take another example, you wouldn't have to translate anything here:

```sh
cd $HOME
rm -rf celestia-app
git clone https://github.com/celestiaorg/celestia-app.git
cd celestia-app/
APP_VERSION=$(curl -s \
  https://api.github.com/repos/celestiaorg/celestia-app/releases/latest \
  | jq -r ".tag_name")
git checkout tags/$APP_VERSION -b $APP_VERSION
make install
```

Furthermore, you do not have to translate URLs into your local language.

#### Specific words

As you will translate innovative concepts, like Data Availability Sampling, feel free to discuss about the best translation with the rest of the community.

Also, be careful with date order, period and commas regarding numbers from a language to another.
