# Go Style Decisions (การตัดสินใจเรื่องสไตล์)

[ภาพรวม](index_th) | [คู่มือหลัก](guide_th) | [การตัดสินใจ](decisions_th) | [แนวทางปฏิบัติที่ดีที่สุด](best-practices_th)

---

**หมายเหตุ:** เอกสารนี้เป็น **เกณฑ์ปฏิบัติ (Normative)** แต่ **ไม่ใช่มาตรฐานถาวร (Canonical)** หากมีข้อขัดแย้งกับ [คู่มือหลัก (Guide)](guide) ให้ยึดตามคู่มือหลักเป็นสำคัญ

## เกี่ยวกับ

เอกสารนี้ประกอบด้วยการตัดสินใจด้านสไตล์ที่มุ่งสร้างความเป็นอันหนึ่งอันเดียวกันและให้คำแนะนำที่เป็นมาตรฐาน คำอธิบาย และตัวอย่างสำหรับคำแนะนำที่ได้รับจากผู้ให้คำปรึกษาด้านความสามารถในการอ่านโค้ด Go (Go readability mentors)

เอกสารนี้ **ไม่ได้ครอบคลุมทุกเรื่อง** และจะเพิ่มพูนขึ้นเรื่อย ๆ เมื่อเวลาผ่านไป ในกรณีที่ คู่มือสไตล์หลัก (the core style guide) ขัดแย้งกับคำแนะนำที่ระบุไว้ ณ ที่นี้ ให้ถือว่า คู่มือสไตล์มีความสำคัญเหนือกว่า และเอกสารนี้ควรได้รับการปรับปรุงแก้ไขให้สอดคล้องกัน

โปรดดูที่ [ภาพรวม](https://google.github.io/styleguide/go#about) สำหรับชุดเอกสาร Go Style ฉบับเต็ม

หัวข้อต่อไปนี้ได้ถูกย้ายจาก style decisions ไปยังส่วน guide:

- **MixedCaps**: see [guide#mixed-caps](guide#mixed-caps)
  <a id="mixed-caps"></a>

- **Formatting**: see [guide#formatting](guide#formatting)
  <a id="formatting"></a>

- **Line Length**: see [guide#line-length](guide#line-length)
  <a id="line-length"></a>

<a id="naming"></a>

## การตั้งชื่อ (Naming)

### การใช้เครื่องหมายขีดล่าง (Underscores)

โดยทั่วไปชื่อใน Go **ไม่ควร** มีเครื่องหมายขีดล่าง (`_`) ยกเว้น 3 กรณี:

1.  ชื่อแพ็กเกจที่ถูก import โดย generated code เท่านั้น
2.  ชื่อฟังก์ชัน Test, Benchmark และ Example ในไฟล์ `*_test.go`
3.  Library ระดับล่างที่ทำงานร่วมกับ OS หรือ cgo (เช่น `syscall`)

### ชื่อแพ็กเกจ (Package names)

- ต้องกระชับ ใช้ตัวพิมพ์เล็กทั้งหมด และไม่มีขีดล่าง (เช่น `tabwriter` ไม่ใช่ `TabWriter` หรือ `tab_writer`)
- หลีกเลี่ยงชื่อที่ซ้ำกับตัวแปรทั่วไป (เช่น `usercount` ดีกว่า `count`)
- หลีกเลี่ยงชื่อที่ไม่สื่อความหมาย เช่น `util`, `common`, `helper`

### ชื่อ Receiver (Receiver names)

- ควรใช้ตัวอักษรเพียง หนึ่งหรือสองตัวอักษร เท่านั้น
- ควรใช้ตัวอักษรที่เป็น คำย่อ หรือตัวย่อของชื่อ Type นั้น ๆ เอง (เช่น `func (c *Client)` ไม่ใช่ `func (cl *Client)`)
- ใช้ชื่อเดิมอย่างสม่ำเสมอตลอดทั้ง สำหรับ Receiver ทุกตัวของ Type เดียวกันนั้น ๆ (เช่น (u User) ก็ควรเป็นแบบนี้ตลอด)

| Long Name                   | Better Name               |
| --------------------------- | ------------------------- |
| `func (tray Tray)`          | `func (t Tray)`           |
| `func (info *ResearchInfo)` | `func (ri *ResearchInfo)` |
| `func (this *ReportWriter)` | `func (w *ReportWriter)`  |
| `func (self *Scanner)`      | `func (s *Scanner)`       |

[Receiver]: https://golang.org/ref/spec#Method_declarations

### ชื่อค่าคงที่ (Constant names)

- ต้องใช้รูปแบบ [MixedCaps] เหมือนกับชื่ออื่นๆ ทั้งหมดในภาษา Go.(เช่น `MaxPacketSize` ไม่ใช่ `MAX_PACKET_SIZE`)
- [Exported] ค่าคงที่เริ่มต้นด้วยตัวพิมพ์ใหญ่ ในขณะที่ค่าคงที่ที่ไม่ได้ส่งออกจะเริ่มต้นด้วยตัวพิมพ์เล็ก
- ค่าคงที่ที่ [Exported] (Exported) จะเริ่มต้นด้วยตัวพิมพ์ใหญ่ (Uppercase) ในขณะที่ค่าคงที่ที่ ไม่ได้ส่งออก (Unexported) จะเริ่มต้นด้วยตัวพิมพ์เล็ก (Lowercase)
  หลักการนี้ใช้ได้แม้ว่ามันจะขัดกับธรรมเนียมปฏิบัติในภาษาอื่น ๆ ก็ตาม ชื่อค่าคงที่ไม่ควรเป็นเพียงการดัดแปลงมาจากค่าของมัน แต่ควรอธิบายว่าค่านั้นสื่อถึงอะไรแทน
  อธิบายว่าค่านั้นสื่อถึงอะไร

```go
// Good:
const MaxPacketSize = 512

const (
    ExecuteBit = 1 << iota
    WriteBit
    ReadBit
)
```

[MixedCaps]: guide#mixed-caps
[Exported]: https://tour.golang.org/basics/3

อย่าใช้ชื่อค่าคงที่ที่ไม่ใช่ MixedCaps หรือค่าคงที่ที่มีคำนำหน้า `K`

```go
// Bad:
const MAX_PACKET_SIZE = 512
const kMaxBufferSize = 1024
const KMaxUsersPergroup = 500
```

ตั้งชื่อค่าคงที่ตามบทบาท ไม่ใช่ตามค่า หากค่าคงที่ไม่มีบทบาทนอกเหนือจากค่าของมัน ก็ไม่จำเป็นต้องนิยามให้เป็นค่าคงที่

```go
// Bad:
const Twelve = 12

const (
    UserNameColumn = "username"
    GroupColumn    = "group"
)
```

<!--#include file="/go/g3doc/style/includes/special-name-exception.md"-->

<a id="initialisms"></a>

### ตัวย่อ (Initialisms)

<a id="TOC-Initialisms"></a>

คำที่ใช้เป็นชื่อย่อหรือตัวอักษรย่อ (Initialisms or Acronyms) เช่น (URL และ NATO) ควรใช้ตัวพิมพ์เดียวกันทั้งหมด

- URL ควรปรากฏเป็น URL หรือ url (เช่น ใน urlPony หรือ URLPony) ไม่ควรเป็น Url
- ตามกฎทั่วไป ตัวระบุ (Identifiers) เช่น (ID และ DB) ควรใช้ตัวพิมพ์ใหญ่ในลักษณะเดียวกับการใช้คำเหล่านั้นในภาษาอังกฤษทั่วไป (English prose)

* ในชื่อที่มีชื่อย่อหลายตัว (Multiple Initialisms) (เช่น `XMLAPI` เนื่องจากประกอบด้วย `XML` และ `API`)
  ตัวอักษรแต่ละตัวภายในชื่อย่อตัวหนึ่ง ๆ ควรใช้ตัวพิมพ์เดียวกันแต่ชื่อย่อแต่ละตัวในชื่อนั้นไม่จำเป็นต้องใช้ตัวพิมพ์เดียวกันทั้งหมด

* ในชื่อที่มีชื่อย่อซึ่งประกอบด้วยตัวอักษรพิมพ์เล็กอยู่ด้วย (เช่น `DDoS`, `iOS`, `gRPC`) ชื่อย่อนั้นควรปรากฏตามรูปแบบที่ใช้ในภาษาอังกฤษทั่วไป (standard prose) ยกเว้น ในกรณีที่คุณจำเป็นต้องเปลี่ยนตัวอักษรตัวแรกเพื่อให้เป็นไปตามกฎของ [exportedness] (การเปิดเผยชื่อให้สามารถใช้งานจากภายนอกแพ็กเกจได้)
  ในกรณีเหล่านี้ ชื่อย่อทั้งหมดควรใช้ตัวพิมพ์เดียวกัน (เช่น `ddos`, `IOS`, `GRPC`)

[exportedness]: https://golang.org/ref/spec#Exported_identifiers

<!-- Keep this table narrow. If it must grow wider, replace with a list. -->

| English Usage | Scope      | Correct  | Incorrect                              |                |
| ------------- | ---------- | -------- | -------------------------------------- | -------------- |
| XML API       | Exported   | `XMLAPI` | `XmlApi`, `XMLApi`, `XmlAPI`, `XMLapi` | ชื่อย่อหลายตัว |
| XML API       | Unexported | `xmlAPI` | `xmlapi`, `xmlApi`                     | ชื่อย่อหลายตัว |
| iOS           | Exported   | `IOS`    | `Ios`, `IoS`                           |                |
| iOS           | Unexported | `iOS`    | `ios`                                  |                |
| gRPC          | Exported   | `GRPC`   | `Grpc`                                 |                |
| gRPC          | Unexported | `gRPC`   | `grpc`                                 |                |
| DDoS          | Exported   | `DDoS`   | `DDOS`, `Ddos`                         |                |
| DDoS          | Unexported | `ddos`   | `dDoS`, `dDOS`                         |                |
| ID            | Exported   | `ID`     | `Id`                                   |                |
| ID            | Unexported | `id`     | `iD`                                   |                |
| DB            | Exported   | `DB`     | `Db`                                   |                |
| DB            | Unexported | `db`     | `dB`                                   |                |
| Txn           | Exported   | `Txn`    | `TXN`                                  |                |

<!--#include file="/go/g3doc/style/includes/special-name-exception.md"-->

<a id="getters"></a>

### Getters

<a id="TOC-Getters"></a>

- ไม่ต้องมีคำว่า `Get` นำหน้า (เช่น `obj.Owner()` ไม่ใช่ `obj.GetOwner()`)
- หากฟังก์ชันเกี่ยวข้องกับการดำเนินการคำนวณที่ซับซ้อน หรือการเรียกใช้จากระยะไกล (remote call) สามารถใช้คำอื่น เช่น Compute (คำนวณ) หรือ Fetch (ดึงข้อมูล) แทน คำว่า Get (รับ/เอา) เพื่อให้ผู้อ่านเข้าใจได้อย่างชัดเจนว่าการเรียกใช้ฟังก์ชันดังกล่าวอาจใช้เวลา, อาจถูกบล็อก (block), หรืออาจล้มเหลวได้

<a id="variable-names"></a>

### ชื่อตัวแปร (Variable names)

<a id="TOC-VariableNames"></a>

กฎทั่วไปที่ใช้เป็นหลักการคือ ความยาวของชื่อควรเป็นสัดส่วนโดยตรงกับขนาดของขอบเขต (scope) ของมัน และเป็นสัดส่วนผกผันกับจำนวนครั้งที่ชื่อนั้นถูกใช้ภายในขอบเขตนั้น

- ตัวแปรที่ถูกสร้างขึ้นที่ ระดับขอบเขตไฟล์ (file scope) อาจต้องการหลายคำ

- ในขณะที่ตัวแปรที่มีขอบเขตจำกัดอยู่แค่ใน บล็อกด้านในเพียงบล็อกเดียว (single inner block) อาจใช้เพียงคำเดียว หรือแม้กระทั่งตัวอักษรเพียงหนึ่งหรือสองตัว เพื่อให้โค้ดมีความชัดเจนและหลีกเลี่ยงข้อมูลที่ไม่จำเป็น (extraneous information)

เกณฑ์คร่าว ๆ ที่ใช้เป็นพื้นฐาน แนวทางตัวเลขเหล่านี้ไม่ใช่กฎที่เข้มงวด ให้ใช้ดุลยพินิจโดยอ้างอิงจากบริบท (context), [ความชัดเจน (clarity)], และ [ความกระชับ (concision)]

- ขอบเขตขนาดเล็ก (A small scope): คือขอบเขตที่มีการดำเนินการเล็ก ๆ น้อย ๆ หนึ่งหรือสองรายการ เช่น ประมาณ 1-7 บรรทัด
- ขอบเขตขนาดกลาง (A medium scope): คือขอบเขตที่มีการดำเนินการขนาดเล็กไม่กี่รายการ หรือการดำเนินการขนาดใหญ่หนึ่งรายการ เช่น ประมาณ 8-15 บรรทัด
- ขอบเขตขนาดใหญ่ (A large scope): คือขอบเขตที่มีการดำเนินการขนาดใหญ่หนึ่งรายการหรือไม่กี่รายการ เช่น ประมาณ 15-25 บรรทัด
- ขอบเขตที่ใหญ่มาก (A very large scope): คือขอบเขตที่ครอบคลุมพื้นที่มากกว่าหนึ่งหน้ากระดาษ (เช่น มากกว่า 25 บรรทัด)

[clarity]: guide#clarity
[concision]: guide#concision

ชื่อที่อาจจะชัดเจนสมบูรณ์แบบ (ตัวอย่างเช่น c สำหรับตัวนับ หรือ counter) ภายใน ขอบเขตขนาดเล็ก อาจไม่เพียงพอใน ขอบเขตที่ใหญ่กว่า และจะต้องการคำอธิบายเพิ่มเติมเพื่อย้ำเตือนผู้อ่านถึงวัตถุประสงค์ของมันเมื่ออ่านโค้ดต่อไปเรื่อย ๆ
ขอบเขตที่มีตัวแปรจำนวนมาก หรือมีตัวแปรที่แสดงถึงค่าหรือแนวคิดที่คล้ายคลึงกัน อาจจำเป็นต้องใช้ชื่อตัวแปรที่ยาวกว่าที่ขนาดของขอบเขตนั้นแนะนำไว้

ความจำเพาะของแนวคิด (The specificity of the concept) ก็สามารถช่วยให้ชื่อตัวแปรมีความกระชับได้เช่นกัน

- ตัวอย่าง: สมมติว่ามีการใช้งานฐานข้อมูลเพียงรายการเดียว (single database) ชื่อตัวแปรสั้น ๆ เช่น db ซึ่งตามปกติจะสงวนไว้สำหรับขอบเขตที่เล็กมาก อาจยังคงชัดเจนสมบูรณ์แบบได้ แม้ว่าขอบเขตนั้นจะใหญ่มากก็ตาม

- ในกรณีนี้ การใช้คำเดียวว่า database ก็อาจเป็นที่ยอมรับได้โดยอิงตามขนาดของขอบเขต แต่ก็ไม่จำเป็นต้องใช้ เนื่องจาก db เป็นคำย่อที่ใช้กันทั่วไปสำหรับคำนี้ และมีการตีความที่แตกต่างกันน้อยมาก

ชื่อของตัวแปรโลคัล (local variable) ควรสะท้อนถึง สิ่งที่ตัวแปรนั้นบรรจุอยู่ และวิธีการใช้งานในบริบทปัจจุบัน มากกว่าที่จะสะท้อนถึงแหล่งที่มาของค่านั้น

ตัวอย่าง: บ่อยครั้งที่ชื่อตัวแปรโลคัลที่ดีที่สุด มักจะไม่ใช่ชื่อเดียวกับชื่อฟิลด์ของโครงสร้าง (struct) หรือชื่อฟิลด์ของ Protocol Buffer

แนวทางการตั้งชื่อโดยทั่วไป

- ชื่อคำเดียว: ชื่อคำเดียว เช่น count (นับ) หรือ options (ตัวเลือก) เป็นจุดเริ่มต้นที่ดี

- การเพิ่มคำ: สามารถเพิ่มคำอื่น ๆ เข้าไปเพื่อแยกแยะชื่อที่คล้ายกันได้ ตัวอย่างเช่น userCount และ projectCount

- ห้ามละเว้นตัวอักษร: อย่าลดทอนตัวอักษรเพียงเพื่อให้พิมพ์ได้สั้นลง ตัวอย่างเช่น ควรใช้ Sandbox มากกว่า Sbx โดยเฉพาะอย่างยิ่งสำหรับชื่อที่ถูก Export (เรียกใช้จากภายนอก)

- ละเว้น [ชนิดข้อมูลและคำที่คล้ายชนิดข้อมูล]: ควรงดเว้นชนิดข้อมูลและคำที่คล้ายชนิดข้อมูลออกจากชื่อตัวแปรส่วนใหญ่

  - สำหรับตัวเลข: userCount เป็นชื่อที่ดีกว่า numUsers หรือ usersInt

  - สำหรับสไลซ์ (Slice): users เป็นชื่อที่ดีกว่า userSlice

- สามารถรวมคำบ่งชี้ที่คล้ายชนิดข้อมูลได้ หากมีค่าสองเวอร์ชันอยู่ในขอบเขตเดียวกัน ตัวอย่างเช่น คุณอาจเก็บค่าที่ป้อน (input) ใน ageString และใช้ age สำหรับค่าที่ถูกแยกวิเคราะห์ (parsed value) แล้ว

- ละเว้นคำที่ชัดเจนอยู่แล้วจาก [บริบทรอบข้าง]: ตัวอย่างเช่น ในการนำไปใช้ของเมธอด UserCount ตัวแปรโลคัลที่ชื่อว่า userCount อาจซ้ำซ้อนเกินความจำเป็น การใช้ count, users, หรือแม้กระทั่ง c ก็สามารถอ่านเข้าใจได้ไม่ต่างกัน

[types and type-like words]: #repetitive-with-type
[surrounding context]: #repetitive-in-context

<a id="v"></a>

### การทำซ้ำ (Repetition)

<!--
Note to future editors:

Do not use the term "stutter" to refer to cases when a name is repetitive.
-->

ควรหลีกเลี่ยงการทำซ้ำที่ไม่จำเป็น (unnecessary repetition)
สาเหตุที่พบบ่อยประการหนึ่งคือ ชื่อที่มีการทำซ้ำ (repetitive names) ซึ่งมักจะรวมคำที่ไม่จำเป็น หรือทำซ้ำบริบทหรือชนิดข้อมูลของตัวเอง
นอกจากนี้ ตัวโค้ดเองก็อาจมีการทำซ้ำที่ไม่จำเป็นได้ หากมีส่วนของโค้ดที่เหมือนกันหรือคล้ายคลึงกันปรากฏหลายครั้งในบริเวณที่ใกล้เคียงกัน

การตั้งชื่อที่ซ้ำซ้อนสามารถมาได้ในหลายรูปแบบ ซึ่งรวมถึง:

<a id="repetitive-with-package"></a>

#### Package vs. exported symbol name

เมื่อตั้งชื่อสัญลักษณ์ที่ถูก Export (ชื่อที่เรียกใช้ได้จากภายนอกแพ็กเกจ) ชื่อของแพ็กเกจจะสามารถมองเห็นได้เสมอจากภายนอกแพ็กเกจของคุณ ดังนั้น ข้อมูลที่ซ้ำซ้อนระหว่างชื่อสัญลักษณ์กับชื่อแพ็กเกจควรลดลงหรือถูกกำจัดออกไป

หากแพ็กเกจหนึ่ง Export ชนิดข้อมูล (type) เพียงชนิดเดียว และชนิดข้อมูลนั้นถูกตั้งชื่อตามชื่อแพ็กเกจเอง ชื่อที่เป็นแบบแผน (canonical name) สำหรับฟังก์ชัน Constructor (ฟังก์ชันสร้างอ็อบเจกต์) คือ `New` หากจำเป็นต้องมีฟังก์ชันดังกล่าว

- **Package vs Exported Symbol:** อย่าตั้งชื่อซ้ำกับแพ็กเกจ (เช่น `widget.New` ดีกว่า `widget.NewWidget`)
- **Context:** อย่าใส่ข้อมูลที่รู้อยู่แล้วจากบริบท (เช่น ในเมธอด `User.Name()` ไม่ต้องเป็น `User.UserName()`)

> **Examples:** Repetitive Name -> Better Name
>
> - `widget.NewWidget` -> `widget.New`
> - `widget.NewWidgetWithName` -> `widget.NewWithName`
> - `db.LoadFromDatabase` -> `db.Load`
> - `goatteleportutil.CountGoatsTeleported` -> `gtutil.CountGoatsTeleported`
>   or `goatteleport.Count`
> - `myteampb.MyTeamMethodRequest` -> `mtpb.MyTeamMethodRequest` or
>   `myteampb.MethodRequest`

<a id="repetitive-with-type"></a>

#### Variable name vs. type

คอมไพเลอร์ (Compiler) ทราบชนิดข้อมูลของตัวแปรอยู่เสมอ และในกรณีส่วนใหญ่ ผู้อ่านก็สามารถเข้าใจได้อย่างชัดเจนว่าตัวแปรนั้นมีชนิดข้อมูลอะไรจากการใช้งานของมัน

การระบุชนิดข้อมูลของตัวแปรให้ชัดเจนนั้น จำเป็นก็ต่อเมื่อ ค่าของตัวแปรนั้นปรากฏซ้ำกันสองครั้งในขอบเขต (scope) เดียวกันเท่านั้น

| Repetitive Name               | Better Name            |
| ----------------------------- | ---------------------- |
| `var numUsers int`            | `var users int`        |
| `var nameString string`       | `var name string`      |
| `var primaryProject *Project` | `var primary *Project` |

หากค่า (value) นั้นปรากฏในหลายรูปแบบ สามารถทำให้ชัดเจนได้โดยใช้คำพิเศษเพิ่มเติม เช่น raw (ดิบ/ต้นฉบับ) และ parsed (ที่ถูกแยกวิเคราะห์แล้ว) หรือโดยการระบุการแสดงผล (representation) :

```go
// Good:
limitRaw := r.FormValue("limit")
limit, err := strconv.Atoi(limitRaw)
```

```go
// Good:
limitStr := r.FormValue("limit")
limit, err := strconv.Atoi(limitStr)
```

<a id="repetitive-in-context"></a>

#### External context vs. local names

ชื่อที่รวมข้อมูลที่ได้มาจากบริบทรอบข้างของมัน มักจะสร้างความรกรุงรัง (extra noise) โดยไม่มีประโยชน์

ชื่อแพ็กเกจ, ชื่อเมธอด, ชื่อชนิดข้อมูล (type name), ชื่อฟังก์ชัน, เส้นทางการนำเข้า (import path), และแม้แต่ชื่อไฟล์ ล้วนสามารถให้บริบทที่มีคุณสมบัติในการระบุชื่อทั้งหมดที่อยู่ภายในนั้นได้โดยอัตโนมัติ

```go
// Bad:
// In package "ads/targeting/revenue/reporting"
type AdsTargetingRevenueReport struct{}

func (p *Project) ProjectName() string
```

```go
// Good:
// In package "ads/targeting/revenue/reporting"
type Report struct{}

func (p *Project) Name() string
```

```go
// Bad:
// In package "sqldb"
type DBConnection struct{}
```

```go
// Good:
// In package "sqldb"
type Connection struct{}
```

```go
// Bad:
// In package "ads/targeting"
func Process(in *pb.FooProto) *Report {
    adsTargetingID := in.GetAdsTargetingID()
}
```

```go
// Good:
// In package "ads/targeting"
func Process(in *pb.FooProto) *Report {
    id := in.GetAdsTargetingID()
}
```

การทำซ้ำควรได้รับการประเมินโดยทั่วไปในบริบทของผู้ใช้งานสัญลักษณ์นั้น ๆ มากกว่าที่จะประเมินแบบแยกส่วน

ตัวอย่างเช่น โค้ดต่อไปนี้มีชื่อจำนวนมากที่อาจจะใช้ได้ในบางสถานการณ์ แต่กลับกลายเป็นการซ้ำซ้อนเมื่ออยู่ในบริบท (redundant in context):

```go
// Bad:
func (db *DB) UserCount() (userCount int, err error) {
    var userCountInt64 int64
    if dbLoadError := db.LoadFromDatabase("count(distinct users)", &userCountInt64); dbLoadError != nil {
        return 0, fmt.Errorf("failed to load user count: %s", dbLoadError)
    }
    userCount = int(userCountInt64)
    return userCount, nil
}
```

แต่ในทางกลับกัน ข้อมูลเกี่ยวกับชื่อที่ชัดเจนอยู่แล้วจากบริบทหรือการใช้งาน มักจะสามารถละเว้นได้:

Instead, information about names that are clear from context or usage can often
be omitted:

```go
// Good:
func (db *DB) UserCount() (int, error) {
    var count int64
    if err := db.Load("count(distinct users)", &count); err != nil {
        return 0, fmt.Errorf("failed to load user count: %s", err)
    }
    return int(count), nil
}
```

<a id="commentary"></a>

## คอมเมนต์ (Commentary)

## -----------check point----------

- **Doc Comments:** ชื่อที่ Export ต้องมี Doc comment เสมอ โดยเริ่มด้วยชื่อของสิ่งนั้นและเป็นประโยคที่สมบูรณ์
  - เช่น `// Request represents a request to run a command.`
- **Package Comments:** ต้องอยู่เหนือ `package` clause โดยไม่มีบรรทัดว่างคั่น

---

## การนำเข้า (Imports)

### การเปลี่ยนชื่อ (Import renaming)

- เปลี่ยนชื่อเฉพาะเมื่อจำเป็น (เช่น ชื่อซ้ำ) หรือเพื่อความอ่านง่าย
- Generated protobuf **ต้อง** เปลี่ยนชื่อให้ลงท้ายด้วย `pb`
- ถ้าชื่อแพ็กเกจไม่สื่อความหมาย (เช่น `v1`) สามารถเปลี่ยนชื่อได้

### การจัดกลุ่ม (Import grouping)

เรียงลำดับดังนี้:

1.  Standard library
2.  Project และ Third-party packages
3.  Protocol Buffer imports
4.  Side-effects imports (`_`)

### Import "dot" (`import .`)

- **ห้ามใช้** ใน Google codebase เพราะทำให้สับสนที่มาของฟังก์ชัน

---

## ข้อผิดพลาด (Errors)

### การคืนค่า Error (Returning errors)

- ใช้ `error` เป็นค่าคืนกลับตัวสุดท้าย
- คืนค่า `nil` เมื่อไม่มี error
- **ห้าม** คืนค่า Concrete error type (เช่น `*os.PathError`) เพราะอาจเกิดปัญหา `nil` interface

### ข้อความ Error (Error strings)

- ไม่ควรขึ้นต้นด้วยตัวใหญ่และไม่ควรมีเครื่องหมายจบประโยค (เพราะมักถูกนำไปต่อกับข้อความอื่น)
  - เช่น `fmt.Errorf("something bad happened")`

### การจัดการ Error (Handle errors)

- อย่าทิ้ง error ด้วย `_`
- จัดการทันที, คืนค่ากลับไป, หรือ `log.Fatal`/`panic` ในกรณีร้ายแรง
- ใช้ Indent เพื่อแยกส่วน Error handling ออกจากโค้ดปกติ (Error อยู่ใน `if`, โค้ดปกติอยู่นอก `else`)

### In-band errors

- หลีกเลี่ยงการคืนค่าพิเศษ (เช่น -1 หรือ null) เพื่อบอก error ให้คืนค่า `error` หรือ `bool` เพิ่มอีกตัวแทน
  - เช่น `func Lookup(key string) (string, bool)`

---

## ภาษา (Language)

### การจัดรูปแบบ Literal (Literal formatting)

- **Field names:** Struct literal ที่ใช้ type จากแพ็กเกจอื่น **ต้อง** ระบุชื่อ field เสมอ
- **Zero-value:** ละ field ที่เป็น zero-value ได้ถ้าไม่ทำให้สับสน
- **Cuddled braces:** การเขียน `}, {` ในบรรทัดเดียว ทำได้เฉพาะเมื่อข้างในเป็น literal หรือ proto builder และ indent ตรงกัน

### Nil slices

- `nil` slice และ empty slice (`[]int{}`) มีพฤติกรรมเหมือนกันในคนส่วนใหญ่
- ชอบใช้ `var s []int` (เป็น nil) มากกว่า `s := []int{}`
- อย่าสร้าง API ที่แยกความแตกต่างระหว่าง nil กับ empty slice

### Indentation confusion

- หลีกเลี่ยงการตัดบรรทัดที่ทำให้ indent ของเงื่อนไขตรงกับ indent ของ block code ข้างใน

### Function formatting

- Signature ของฟังก์ชันควรอยู่ในบรรทัดเดียวถ้าทำได้
- ถ้าต้องตัดบรรทัด argument ให้ตัดตามกลุ่มความหมาย ไม่ใช่แค่เพราะความยาว

### Conditionals & Loops

- `if` ไม่ควรตัดบรรทัด
- `switch` และ `case` ควรอยู่บรรทัดเดียว
- **Yoda style:** เลี่ยงการเขียน `if "foo" == result` ให้เขียน `if result == "foo"`

### การคัดลอก (Copying)

- ระวังการ copy struct ที่มี `sync.Mutex` หรือ `bytes.Buffer` (เพราะอาจเกิด bugs)
- ถ้า struct ไม่ควรถูก copy ให้ใช้ pointer receiver

### Panic

- อย่าใช้ `panic` สำหรับ error ปกติ ให้ใช้ `error`
- ใช้ `panic` หรือ `log.Fatal` เฉพาะกับข้อผิดพลาดที่ "เป็นไปไม่ได้" หรือตอนเริ่มโปรแกรม (init)

### Must functions

- ฟังก์ชัน `MustXYZ` (เช่น `MustParse`) ใช้สำหรับช่วย setup ที่จะ panic ถ้าทำงานไม่สำเร็จ
- ควรใช้เฉพาะตอนเริ่มโปรแกรม (init) หรือใน Tests

### Interfaces

- Interface ควรนิยามในแพ็กเกจที่ **ใช้** (Consumer) ไม่ใช่แพ็กเกจที่ทำ (Implementer)
- รับ Interface คืน Concrete type

### Generics

- ใช้อย่างระมัดระวัง อย่าใช้แค่เพราะ "ทำได้"
- ถ้า Code ทำงานกับ Type เดียว ไม่ต้องใช้ Generics

### Pass Values vs Pointers

- อย่าส่ง Pointer เพียงเพื่อประหยัด Memory เล็กน้อย (เช่น `*string`, `*int`)
- Proto buffer ควรส่งเป็น Pointer เสมอ

### Receiver Type

- ถ้า method ต้องแก้ค่าใน receiver -> ใช้ **Pointer**
- ถ้า struct ห้าม copy (เช่นมี Mutex) -> ใช้ **Pointer**
- ถ้าเป็น slice, map, channel, function -> ใช้ **Value**
- ถ้าเป็น simple basic type (int, string) -> ใช้ **Value**
- ถ้าไม่แน่ใจ -> ใช้ **Pointer**

---

## ไลบรารีทั่วไป (Common libraries)

- **Flags:** ชื่อ Flag ใน command line ใช้ `snake_case` แต่ชื่อตัวแปรใช้ `CamelCase`
- **Contexts:**
  - `ctx context.Context` ต้องเป็น parameter แรกเสมอ
  - ห้ามเก็บ Context ใน Struct
  - ห้ามสร้าง Custom Context type
- **Crypto/rand:** ใช้ `crypto/rand` สำหรับสร้าง key ห้ามใช้ `math/rand`

---

## การทดสอบ (Useful Test Failures)

- **Assertion Libraries:** ห้ามสร้างหรือใช้ library assertion ให้ใช้ Go ปกติ (`if got != want`) หรือไลบรารีมาตรฐานเช่น `cmp`
- **Got before Want:** พิมพ์ค่าที่ได้ก่อนค่าที่คาดหวัง (`got %v, want %v`)
- **Print Diffs:** ถ้า output ใหญ่หรือดูยาก ให้ใช้ `cmp.Diff` เพื่อแสดงความแตกต่าง
- **Table-driven tests:** แนะนำให้ใช้เมื่อมี test cases หลายกรณีที่มี logic คล้ายกัน
- **Test Helpers:** ใช้ `t.Helper()` เพื่อให้ error log ชี้ไปที่บรรทัดที่เรียกใช้ helper ไม่ใช่ในตัว helper เอง
- **Package `testing`:** ใช้แพ็กเกจ `testing` เท่านั้น ห้ามใช้ Framework อื่น

---
