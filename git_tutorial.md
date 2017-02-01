
#Một số kỹ năng nâng cao trong git
##Khái niệm version control
Version Control là một hệ thống ghi nhận lại các sự thay đổi qua thời gian của một hoặc 1 tập các file, đo đó sau này bạn có thể xem lại các phiên bản xác định của các file này. Với Git, bạn sẽ sử dụng mã nguồn của phần mềm như là các file sẽ được quản lý phiên bản, mặc dù trong thực tế bạn có thể sử dụng Git để quản lý gần như mọi loại file có trong máy tính.
Hệ thống Version Control cho phép bạn xem lại các trạng thái trước đó của một file, chuyển các file về trạng thái trước đó của nó, so sánh giữa các trạng thái với nhau, người tạo ra các trạng thái.... Đồng thời version control cho phép người dùng có thể dễ dàng phục hồi các file về trạng thái trước đó của nó, nếu bạn có lỡ thay đổi hoặc xóa nó. Tất cả các tác vụ trên đều được thực hiện với chi phí xử lý và lưu trữ nhỏ nhất.

##Git basics

###Các đặc điểm khác biệt của Git so với các VCS khác
Việc bạn hiểu được **git** là gì, và cách thức hoạt động của nó như thế nào là 1 điều rất quan trọng, vì làm được điều này sẽ giúp bạn sử dụng git một cách dễ dàng và hiệu qủa. 

Mặc dù Git có giao diện người dùng  giong với một số hệ thống quản lý phiên bản (VCS) khác, tuy nhiên ở bên trong, nguyên lý hoạt động của Git có phần khác với cac hệ thống này, bên cạnh đó git lưu trữ và xử lý thông tin theo một cách khác biệt so với các VCS khác. Việc hiểu được những sự khác biệt này giúp cho chúng ta tránh được sự  lẫn lộn khi dùng các VCS khác nhau cùng 1 lúc.

**Snapshots, Not Differences**
Sự khác biệt chính giữa Git và các VCS khác là cái cách mà Git nghĩ về dữ liệu mà nó quản lý. Một cách tổng quát, hầu hết các VCS khác đều lưu trữ dữ liệu như là một danh sách thay đổi của file. Các hệ thống này nhìn thông tin mà chúng lưu trữ như là 1 danh sách các file và các thay đổi trên các file đó theo thời gian. 
![data stores in other VCS](https://git-scm.com/book/en/v2/images/deltas.png)
Git không nhìn nhận và lưu trữ dữ liệu theo cách đó. Git nhìn nhận dữ liệu mà nó lưu trữ như là một danh sách các snapshot của hệ thống file mà nó quản lý. Bất cứ khi nào bạn commit hoặc lưu trạng thái project của bạn trên git, một cách đơn giản là git sẽ tạo ra 1 khung nhìn, cho phép nhìn thấy trạng thái và nội dung của tất cả các file của bạn tại thời điểm hiện tại, đồng thời lưu trữ tham chiếu trỏ tới khung nhìn vừa được tạo ra.Để tăng tính hiệu qủa, nếu nội dung của file không thay đổi tại thời điểm tạo commit, git không lưu tất cả nội dung của file vào snapshot mà chỉ lưu tham chiếu tới nội dung của file tại lần cuối cùng nó bị sửa đổi trước commit, Git nhìn nhận dữ liệu như là một dòng chảy (stream) các snapshot. 
![git- stream of snapshots](https://git-scm.com/book/en/v2/images/snapshots.png)

**Nearly Every Operation Is Local**
Gần như hầu hết các tính năng mà Git cung cấp chỉ cần đến các file và tài nguyên tại local để thực hiện, nghĩa là hầu hết các chức năng mà git cung cấp không cần có sự tham gia của các máy tính khác trong mạng. Nếu bạn sử dụng CVCS (central version control system), hầu hết các thao tác của bạn sẽ chịu ảnh hưởng từ độ trễ (network latency) của mạng, điều này khiến cho hiệu năng của Git là rất tốt. Bởi vì bạn có toàn bộ dữ liệu về lịch sử của dự án của bạn ở local disks, mọi thao tác được thực hiện gần như tức thì. 

**Git Has Integrity**
Tất cả mọi thứ mà Git quản lý đều được kiểm tra lỗi -  **check-summed**
trước khi chúng được lưu trữ, và sau đó bị tham chiếu tới bởi chính mã check-sum được tạo ra. Điều đó có nghĩa là không có cách nào để thay đổi nội dung của file hoặc thư mục mà không làm  git biết được sự thay đổi đó. Chức năng này được xây dựng trong git ở tầng thấp nhất và được tích hợp vào trong nguyên lý thiết kế **git**. Bạn không thể mất thông tin trong khi vận chuyển hoặc lấy về một file lỗi vì git có thể phát hiện ra các file lỗi này.
Git thực hiện việc checksum bằng cách sử dụng mã SHA1 hash. Khi sử dụng  mã hash này, nội dung của file hay thư mục được checksum sẽ được tham chiếu bởi một dãy 40 ký tự hexadecimal. Do đó bạn sẽ thấy mã này xuất hiện ở mọi nơi trong git. Trên thực tế để quản lý hệ thống file và thư mục, git không những lưu tên của file/thư mục đó mà còn lưu trữ cả mã hash tham chiếu tới file/thư mục đó. 

**Git Generally Only Adds Data**
Khi bạn thực hiện các hành động trong  Project mà **git** quản lý,  gần như là git sẽ chỉ thêm dữ liệu vào cơ sở dữ liệu của nó. Điều này giúp cho
các dữ liệu mà Git quản lý được an toàn, đặc biệt là khi bạn thường xuyên push Project của bạn sang các repository ở các máy khác.
Để hiểu rõ hơn về cách mà Git lưu trữ dữ liệu như thế nào, bạn có thể đọc phần **Undoing Things** trong sách.


###Ba trạng thái trong Git
Bây giờ, bạn cần tập trung chú ý, đây là một điều quan trọng về Git mà bạn cần nhớ nếu bạn muốn từ bây giờ qúa trình đọc về Git trở nên suôn sẻ và trôi chảy. Trong git, một file/folder tồn tại ở 1 trong 3 trạng thái chính: **committed, modified, and staged**. Committed nghĩa là dữ liệu về file đó đã được lưu trữ an toàn trong local database. Modified có nghĩa là bạn đã thay đổi nội dung của file nhưng chưa commit nó vào local database đồng thời cũng  **chưa được đánh dấu là đã được stagged**. Staged có nghĩa là bạn đã đánh dấu một file bị thay đổi sẽ lưu trạng thái hiện tại của nó vào snapshot ở lần commit tiếp theo.
Ba trạng thái này dẫn đến việc trong 1 Git Project sẽ có 3 khu  vực chính:
the Git directory (thư mục .git), the working directory, and the staging area.
![git areas](https://git-scm.com/book/en/v2/images/areas.png)
**Git directory** là nơi mà Git lưu trữ các thông tin siêu dữ liệu và object database cho project của bạn. Đây là thành phần quan trọng nhất trong 1 git Project, và cũng là thứ được sao chép khi bạn clone một repository từ máy khác về máy bạn.

**Working tree**(Working directory) là thể hiện đầy đủ của một version trong project (Lưu ý là project của bạn là 1 tập hợp của rất nhiều các version). Nội dung khu vực này được tạo ra như sau: tất cả các file trong version này sẽ được giải nén từ cơ sở dữ liệu trong **Git directory** và đặt vào một thư mục trong đĩa và cho phép bạn thao tác, xem và sửa đổi các tệp trong version này.
**The staging area** về cơ bản là 1 file, được chứa trong **Git directory**, file này chứa thông tin là **những gì sẽ được lưu vào commit tiếp theo** của bạn.  
Workflow cơ bản của một Git Project như sau:

 1. Bạn thực hiện thay đổi nội dung một số file trong **working directory** của bạn. Các file này sẽ chuyển sang trạng thái **modified**
 2. Bạn thực hiện việc **stag** các file này (bằng các lệnh như **git add** ... ). Khi đó snapshot của các file đã được **stagged** sẽ được lưu vào **stagging area**. 
 3. Bạn thực hiện việc tạo ra 1 commit, hành động này sẽ lấy các thông tin có trong stagged area ra và tạo ra 1 **version** mới cho project của bạn.

Nếu nội dung của một file trong **working directory** không thay đổi so với version tương ứng của nó trong **Git directory**, nó sẽ được coi là ở trạng thái **committed**. Nếu 1 file đã được thay đổi nội dung so với version tương ứng trong **Git directory** và được người dùng thực hiện hành động **stag**, tức là được đưa vào trong **staging area**, nó sẽ ở trạng thái **staged**. Nếu 1 file đã được thay đổi nội dung so nhưng chưa được người dùng thực hiện **stag**, nó sẽ ở trạng thái **modified**.
##Recording Changes to the Repository
Bạn có một repository cho project của bạn và working directory chứa một version đã được giải nén của project. Sau đó bạn tạo ra một số thay đổi, một số nội dung mới trong working directory và project của bạn chuyển đến một trạng thái mà bạn muốn ghi (record) lại, lúc này bạn cần commit các snapshots của những sự thay đổi bạn đã tạo ra trong working directory.

**!Important** : Bạn cần ghi nhớ rằng, các file trong working directory tồn tại ở một trong 2 trạng thái: Được kiểm soát (**tracked**) và không được kiểm soát (**untracked**). **Tracked file** là các file đã xuất hiện ở **snapshot - commit cuối cùng**, như đã trình bày ở phần trước, các file này lại có thể thuộc 1 trong 3 trạng thái: unmodified, modified,  staged. **Untracked file** là những thứ còn lại trong working directory: Tất cả những file chưa xuất hiện ở **snapshot cuối cùng**, và cũng không xuất hiện trong **staging area**. Tại sao lại cần tới 2 điều kiện ở đây ? Vì nếu bạn thực hiện thao tác **git add** một file đang ở trạng thái **untracked**, file này sẽ được chuyển vào staging area, đồng thời nó cũng chưa xuất hiện ở **snapshot cuối cùng**. Sau khi bạn clone xong một repository, thì trạng thái của các file trong working directory lúc đó đều sẽ là **tracked - umodified**, vì khi đó các file trong working directory có nguồn gốc từ một version trong project và được git giải nén ra, và bạn chưa thực hiện sửa đổi gì trong **working directory** cả.

Khi bạn sửa đổi (sửa đổi chứ không phải tạo mới) các file trong working directory, git sẽ nhìn nhận các file này ở trạng thái **modified**, vì nội dung của chúng đã thay đổi so với nội dung của chúng ở commit cuối cùng. Bạn thực hiện việc **stag** các file đã bị thay đổi nội dung, sau đó thực hiện **commit** snapshot để tạo ra version mới cho project, và sau đó chu kỳ này sẽ được lặp lại.
![git-life-cycle](https://git-scm.com/book/en/v2/images/lifecycle.png)
###Kiểm tra trạng thái các files của bạn trong working directory
Công cụ chính để bạn kiểm tra trong working directory, file nào đang ở trạng thái nào là câu lệnh ```git status```. Nếu bạn sử dụng câu lệnh này sau khi clone một repository, bạn sẽ thấy kết qủa sau:
```bash
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```
Điều này có nghĩa là working directory của bạn **clean**, hay nói cách khác là không có các file ở trạng thái **tracked** và **modified**, đồng thời cũng không có file nào ở trạng thái **untracked**. Đồng thời kết qủa cũng chỉ ra bạn đang ở branch nào.


##.gitignore - ignoring files
Thông thường, trong project của bạn sẽ xuất hiện một số file mà bạn không muốn Git tự động thêm vào hoặc thậm chí bạn không muốn nó xuất hiện dưới dạng untracked. Đó thường là các file được tự động sinh ra khi bạn chạy hệ thống của bạn (ví dụ như 1 số file sau biên dịch của Python project như **.pyc file**, hoặc là một số file **log** được tạo ra khi bạn build và run project. Trong trường hợp này, bạn có thể bỏ qua những file này bằng cách tạo ra 1 file liệt kê các **pattern match** với các file trên và đặt tên file vừa tạo ra là **.gitignore**
```bash
$ cat .gitignore
*.[oa]
*~
```
Dòng đầu tiên trong file **.gitignore** trên nói cho git biết rằng chúng ta sẽ bỏ qua các file có kết thúc là **.o** hoặc **.a** - đây là các **ojbect file** hoặc là **archive file** được tạo ra khi chúng ta build project. Dòng thứ 2 nói rằng chúng ta sẽ bỏ qua các file có kết thúc là **~**, đây là những file được 1 số editor coi là file tạm (temporary file). Bạn cũng có thể thêm vào **.gitignore** các pattern khác để bỏ qua các thư mục như **log**, **tmp**, **pid**, các file tạm được sinh ra khi hệ thống build project, vv... Thiết lập 1 file **.gitignore** là một ý tưởng tốt khi bắt đầu xây dựng 1 project, vì nó sẽ giúp bạn tránh khỏi việc commit các file mà bạn không muốn xuất hiện trong project lên **Git repository**.
Đây là một số rule về các pattern mà bạn có thể đặt vào trong **.gitignore** file:
- Một dòng rỗng (blank line) hoặc một dòng bắt đầu bằng dấu **#** bị bỏ qua.
- Các standard glob pattern sẽ được thực thi.
- Bạn có thể bắt đầu pattern bằng dấu **/** để không sử dụng đệ quy **recursivity**
- Bạn có thể kết thúc 1 pattern bằng dấu **/** để đánh dấu pattern này áp dụng cho thư mục (directory).
- Bạn có thể đảo ngược ý nghĩa 1 pattern (tức là đây sẽ là 1 **accept pattern** chứ không phải 1 **ignore pattern** bằng cách thêm vào đầu dòng dấu **!**
Glob pattern là một **regular expression** mà shell - terminal thường sử dụng. Dấu ***** đại diện cho 1 hoặc nhiều ký tự. **[abc] ** đại diện cho 1 trong số các ký tự thuộc tập {a, b, c}. Dấu **?** đại diện cho một ký tự. Một cái ngoặc(**[]**) với 2 ký tự **a1**, **b1** bên trong ngăn cách bởi dấu nối (**-**) đại diện cho 1 trong các ký tự trong khoảng [a1-b1]. Ví dụ **[0-9]** đại diện cho 1 trong các ký tự thuộc tập {1, 2, 3, 4, 5, 6, 7, 8, 9}. Bạn có thể sử dụng 2 dấu ***** để đại diện cho 1 tập các thư mục. Ví dụ **a/\**/z** có thể đại diện cho **a/z, a/b/z, a/b/c/z**, vv...

Đây là 1 số ví dụ về glob pattern:
```bash
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```

##Committing Your Changes
Bây giờ, chúng ta gỉa sử rằng thành phần **staging area** đã được thiết lập đúng theo ý muốn của bạn. Bạn nên nhớ rằng, tất cả mọi thứ trong **working directory** mà đang ở trạng thái **unstaged** (**modified** + **untracked**), hay tất cả các file mà bạn đã tạo ra hoặc sửa đổi nhưng chưa được bạn thực hiện hành động **git add**, sẽ không được cho vào commit này. Chúng sẽ vẫn nằm trong **working directory** ở trạng thái **modified** và **untracked**.

Khi bạn đã sẵn sàng lưu các thay đổi mà bạn đã tạo ra thành 1 version mới cho project, bạn tiến hành commit những thay đổi này bằng câu lệnh:
```bash
git commit 
[master 463dc4f] Story 182: Fix benchmarks for speed
2 files changed, 2 insertions(+)
create mode 100644 README

```
Điều bạn cần làm tiếp theo là nhập ghi chú vào cho commit của bạn.

Sau khi git tạo xong version mới cho project, kết qủa chúng ta nhận được là ghi chú đi kèm với version mới, branch mà bạn đang thực hiện. mã SHA tương ứng với **version - commit** mới, bao nhiêu file đã bị thay đổi, bao nhiêu dòng đã được thêm vào, bao nhiêu dòng đã bị xóa đi.

Ghi nhớ rằng thao tác **commit** thực hiện việc ghi lại các **snapshot** của các file đang có trong **staging area**. Tất cả mọi thay đổi mà không được bạn thực hiện thao tác **stag** sẽ vẫn ở trạng thái **modified - untracked**, tất nhiên là bạn có thể thực hiện việc **stag** chúng ( bằng **git add**) để lưu chúng vào một **commit** nào đó sau này. Bất cứ khi nào bạn thực hiện thao tác commit, là bạn đã thực hiện việc tạo ra 1 **version - snapshot - commit** cho project của bạn, và sau này bạn có thể dùng **version - snapshot - commit** này để so sánh (**compare**) hoặc quay trở lại (**revert**) version này bất cứ khi nào bạn muốn. 


**Skipping the Staging Area**
Bạn có thể commit toàn bộ các thay đổi đã tạo ra bằng câu lệnh:
```bash
$ git commit -a -m '#message_content'
```
khi bạn đưa thêm **option -a** vào câu lệnh **git commit**, Git sẽ tự động **stag** tất cả  những sự thay đổi của **các file đang ở trạng thái tracked** trước khi thực hiện commit. Điều này giups bạn không cần phải thực hiện các thao tác **git add** nữa.

Lưu ý, nên cẩn thận khi sử dụng **option** này, vì nó sẽ tác động đến tât cả mọi sự thay đổi đang có trong **working directory**, bao gồm cả những thay đổi mà bạn đã tạo ra tại các file nhưng bạn không muốn lưu lại.

**Removing Files**
Để loại bỏ 1 file khỏi **Git**, bạn cần loại bỏ nó khỏi tập **tracked file** (chính xác hơn, là loại bỏ nó khỏi  **staging area**) (1), sau đó thực hiện việc **commit** (2). Câu lệnh **git rm** được sử dụng để thực hiện công việc (1) , đồng thời sau khi thực hiện công việc này, **git rm** cũng thực hiện luôn việc loại bỏ file này ra khỏi **working directory**, do đó bạn cũng sẽ không thấy file này tồn tại trong **working directory**, và tất nhiên file này cũng sẽ không tồn tại ở trạng thái **untracked**, sau khi git thực hiện xong câu lệnh **git rm**.
Nếu như, bạn chỉ thực hiện việc xóa file trong working directory (ví dụ như chọn tệp -> nhấn **delete**), thì nó sẽ chuyển sang trạng thái **modified - unstaged**. Khi đó, khi bạn thực hiện thao tác kiểm tra **git status**, bạn sẽ thấy git thông báo kết qủa là **Changed but not updated**:
```bash
$ rm PROJECTS.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
(use "git add/rm <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)
deleted:
 PROJECTS.md
no changes added to commit (use "git add" and/or "git commit -a")
```
Bây giờ nếu chúng ta sử dụng câu lệnh **git rm** - **git add**, nó sẽ chuyển file sang trạng thái **staged**, tức là ghi nhận sự biến mất của nó trong **version -commit**  mới.
```bash
$ git rm PROJECTS.md
rm 'PROJECTS.md'
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)
deleted:
 PROJECTS.md
```

Trong lần commit tiếp theo, file bị chúng ta áp dụng thao tác **git rm** sẽ không còn xuất hiện trong cả version mới lẫn trong cả working directory, nếu bạn có thực hiện việc thay đổi file trên so với commit cuối cùng và chuyển nó sang trạng thái **staged** bằng câu lệnh **git add**, bạn phải thêm option **-f (force)** vào câu lệnh **git rm**. Lý do phải thực hiện điều này, vì git muốn đảm bảo an toàn cho dữ liệu của bạn, nếu dữ liệu bạn thay đổi chưa được ghi nhận vào **snapshot** - thì dữ liệu đó không thể được phục hồi bằng git.
Một option hữu ích khác có thể bạn sẽ cần, nếu bạn muốn giữ lại file của bạn trên ổ cứng ở **working directory** nhưng lại không muốn git kiểm soát file này nữa, nghĩa là chuyển file này từ trạng thái **tracked**  sang trạng thái **untracked**, đó là option **-- cached**. Một ví dụ mà trong đó, option này hữu ích là, một lúc nào đó bạn quên thêm một số thứ vào file **.gitignore**, và lỡ **staged** một số file không mong muốn vào **stagging area**, như là một số file log lớn, hoặc một số file là kết qủa sau biên dịch. 

Bạn có thể đưa vào câu lệnh  **git rm** tên tệp, tên thư mục, hoặc một **pattern matching** khớp với 1 loạt các tệp, các thư mục nào đó. Ví dụ:
```bash
$ git rm log/\*.log
```
Ở đây chúng ta cần sử dụng dấu **\\**, vì **\*** là ký tự đặc biệt trong pattern matching. Câu lệnh trên loại  bỏ tất cả các file có đuôi .log trong thư mục log.

**Moving Files**

Git sử dụng câu lệnh **git mv** để thực hiện di chuyển 1 file/folder từ thư mục này trong project  tới thư mục khác trong project:
**$ git mv file_from file_to**
Ví dụ, câu lệnh:
```bash
$ git mv README.md README
```
tương đương với tổ hợp 3 lệnh sau:
```bash
$ mv README.md README
$ git rm README.md
$ git add README
```

##Viewing the Commit History
Sau khi bạn tạo ra hàng loạt các commit, hoặc là bạn đã clone một repository với lịch sử là một loạt các commit đã được tạo ra trong repository trước khi bạn clone, bạn sẽ có thể muốn nhìn trở lại để xem chuyện gì đã xảy ra ở các commit trước đó. Công cụ cơ bản và mạnh mẽ nhất để thực hiện yêu cầu này là câu lệnh **git log**.
```bash
$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:
 Mon Mar 17 21:52:11 2008 -0700
changed the version number
commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:
 Sat Mar 15 16:40:33 2008 -0700
removed unnecessary test
commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:
 Sat Mar 15 10:31:28 2008 -0700
first commit

```
với câu lệnh mặc định, **git log** liệt kê tất cả các commit đã được tạo ra trong repository hiện tại theo thứ tự dảo ngược thời gian - commit gần đây nhất sẽ xuất hiện đầu tiên, vv... 
Có hàng loạt tính năng được tích hợp vào **git log** cho phép bạn xem lại lịch sử các commit, ở đây chúng ta cùng xem một số tính năng sau:

- option **-p**: sử dụng để hiển thị những khác biệt giữa các commit liên tiếp nhau.
- option **-#entries_number**: sử dụng để giới hạn số commit tối đa được hiện lên màn hình.
```bash
$ git log -p -2
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:
 Mon Mar 17 21:52:11 2008 -0700
changed the version number
diff --git a/Rakefile b/Rakefile
index a874b73..8f94139 100644
--- a/Rakefile
+++ b/Rakefile
@@ -5,7 +5,7 @@ require 'rake/gempackagetask'
spec = Gem::Specification.new do |s|
s.platform =
 Gem::Platform::RUBY
s.name
 =
 "simplegit"
-
 s.version
 =
 "0.1.0"
+
 s.version
 =
 "0.1.1"
s.author
 =
 "Scott Chacon"
s.email
 =
 "schacon@gee-mail.com"
s.summary
 =
 "A simple gem for using Git in Ruby code."
commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:
 Sat Mar 15 16:40:33 2008 -0700
removed unnecessary test
diff --git a/lib/simplegit.rb b/lib/simplegit.rb
index a0a60ae..47c6340 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -18,8 +18,3 @@ class SimpleGit
end
end
-
-if $0 == __FILE__
- git = SimpleGit.new
- puts git.show
-end
\ No newline at end of file
```
Để hiển thị thông tin về số lượng các file bị thay đổi trong mỗi commit, sử dụng option **--stat**:
```
$ git log --stat
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:
 Mon Mar 17 21:52:11 2008 -0700
changed the version number
Rakefile | 2 +-
1 file changed, 1 insertion(+), 1 deletion(-)
commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:
 Sat Mar 15 16:40:33 2008 -0700
removed unnecessary test
lib/simplegit.rb | 5 -----
1 file changed, 5 deletions(-)
commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:
 Sat Mar 15 10:31:28 2008 -0700
first commit
README
 | 6 ++++++
Rakefile
 | 23 +++++++++++++++++++++++
lib/simplegit.rb
 | 25 +++++++++++++++++++++++++
3 files changed,
 54 insertions(+)
```
- Sử dụng option **--graph** để hiển thị các commit dưới dạng đồ thị các **branch**
- Để giới hạn các commit theo thời gian, sử dụng các option **--since**, **--until**
- Để tìm kiếm các commit có chứa một nội dung nào đó, sử dụng option **-S**

##Undoing Things
Ở bất kỳ thời điểm nào, bạn cũng có nhu cầu được ``undo`` một số thao tác. Một trong những trường hợp mà bạn muốn **undo** khi sử dụng **git**, đó là khi mà bạn commit qúa sớm, bạn quên add một vài file, hoặc bạn muốn viết lại message cho commit. Nếu bạn muốn thiết lập lại **commit** gần nhất, chúng ta có thể sử dụng option **\-\-amend**:
```bash
$ git commit --amend
```
Câu lệnh này lấy các dữ liệu bên trong **staging area** và sử dụng chúng để sửa lại **commit**. Nếu bạn không thay đổi điều gì so với **commit** cuối cùng, khi đó **snapshot - commit** sẽ không thay đổi về nội dung, và lúc đó cái bạn thay đổi là nội dung của **commit message**.
Một ví dụ, gỉa sử bạn quên thêm những thay đổi của 1 file vào commit, khi đó bạn cần thực hiện các câu lệnh sau để thêm những thay đổi mà bạn quên vào commit:
```bash
#before
$ git commit -m 'initial commit'
#after
$ git add forgotten_file
$ git commit --amend
```
Câu lệnh commit thứ 2 sẽ đè vào commit trước đó.

**Unstaging a Staged File**
2 phần tiếp sau đây sẽ chứng minh cách làm thế nào để bạn sắp xếp sự thay đổi bên trong **staging area** và **working directory**. Ví dụ, bạn vừa thay đổi nội dung 2 file, và bây giờ bạn muốn tạo ra 2 commit riêng rẽ, mỗi commit tương ứng với sự thay đổi của một file, nhưng không may bạn lại thực hiện câu lệnh ```git add *``` và làm cho cả 2 file cùng bị **staged**, bây giờ làm thế nào để chúng ta **unstaged** một trong 2 file ? Lúc này, câu lệnh ```git status``` sẽ gợi ý cho bạn cách giải quyết:
```bash
$ git add *
$ git status
On branch master
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)
renamed:
modified:
README.md -> README
CONTRIBUTING.md
```
Ở đây, có thể thấy git gọi ý chúng ta sử dụng câu lệnh ```git reset HEAD <file>...``` để **unstage** các file đang ở trạng thái **staged**.
```
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M
 CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)
renamed:
 README.md -> README
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)
modified:
 CONTRIBUTING.md
```
Chúng ta có thể thấy, sau câu lệnh trên, file **CONTRIBUTING.md** đã trở lại trạng thái **modified**.

**Unmodifying a Modified File**

Bây giờ, nếu như bạn không muốn giữ lại những thay đổi bạn tạo ra trên file **CONTRIBUTING.md** ở **working area**, bạn phải làm như thế nào ?
Làm thế nào để bạn đưa nội dung file này trở về nội dung của nó ở snapshot cuối cùng? Chúng ta có thể sử dụng gợi ý mà **git status** đã hiển thị:
```bash
$ git status
On branch master
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)
renamed:
 README.md -> README
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)
```
Bạn có thể sử dụng câu lệnh **git checkout** để thực hiện công việc loại bỏ những thay đổi bạn đã tạo ra trên các file trong **working directory**:
```bash
$ git checkout -- CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)
renamed:
 README.md -> README
```
Sau khi câu lệnh này được thực hiện, bạn có thể vào xem lại nội dung của file CONTRIBUTING.md trong **working directory** để kiểm tra kết qủa.
Lưu ý, khi sử dụng câu lệnh này, mọi thay đổi bạn đã tạo ra trên các file bị lệnh này tác động sẽ biến mất, do đó bạn phải cẩn thận khi dùng câu lệnh **git checkout**.

Mọi thay đổi bạn tạo ra mà đã được thực hiện thao tác **committed** đều luôn luôn có thể phục hồi. Điều này chúng ta sẽ tìm hiểu ở chương ```Data Recovery```.
##Working with Remotes
Để có thể cộng tác với người khác trong bất kỳ một project nào, bạn cần biết cách để quản lý remote repository của bạn - repository được đặt trên internet hoặc được đặt ở máy server trên local network của bạn. Bạn sẽ có thể có một loạt các repository, với một số quyền khác nhau, ví dụ một số repository chỉ cho bạn quyền đọc (read-only), một số repository cho bạn quyền đọc/ghi. Sự cộng tác với người khác sẽ kéo theo việc bạn cần quản lý các remote repositories này, và thực hiện việc **push data** hoặc **pull data** tới các remote repositories khi bạn cần chia sẻ công việc của bạn với những người khác.

Quản lý remote repository bao gồm những thao tác như add một remote repository, xóa bỏ một remote repository không còn phù hợp (valid), quản lý một loạt các **remote branch** và định nghĩa xem chúng có được **tracked** hay không.

**Showing Your Remotes**
Để kiểm tra xem bạn đang kết nối với những remote repositories nào, sử dụng câu lệnh **git remote**
```bash
$ git clone https://github.com/schacon/ticgit
Cloning into 'ticgit'...
remote: Reusing existing pack: 1857, done.
remote: Total 1857 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (1857/1857), 374.35 KiB | 268.00 KiB/s, done.
Resolving deltas: 100% (772/772), done.
Checking connectivity... done.
$ cd ticgit
$ git remote
origin
```
option **-v** cho phép chúng ta xem URL tương ứng với từng remote repository:
```bash
$ git remote -v
origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)
```
Trường hợp project kết nối với nhiều repositories:
```bash
$ cd grit
$ git remote -v
bakkdoor https://github.com/bakkdoor/grit (fetch)
bakkdoor https://github.com/bakkdoor/grit (push)
cho45
 https://github.com/cho45/grit (fetch)
cho45
 https://github.com/cho45/grit (push)
defunkt
 https://github.com/defunkt/grit (fetch)
defunkt
 https://github.com/defunkt/grit (push)
koke
 git://github.com/koke/grit.git (fetch)
koke
 git://github.com/koke/grit.git (push)
origin
 git@github.com:mojombo/grit.git (fetch)
origin
 git@github.com:mojombo/grit.git (push)
```

**Adding Remote Repositories**
Để thêm một remote repositories vào project, chúng ta sử dụng câu lệnh ```git remote add <shortname> <url>```
```bash
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)
pb
 https://github.com/paulboone/ticgit (fetch)
pb
 https://github.com/paulboone/ticgit (push)
```
Bây giờ bạn chỉ cần sử dụng tên của remote repository này là **pb** thay vì phải sử dụng cả URL dài của nó - **https://github.com/paulboone/ticgit** . Ví dụ nếu bạn muốn lấy các thông tin mà Paul đã đẩy lên **remote repository** trên, nhưng bạn lại chưa có nó trong **local repository**, bạn có thể sử dụng câu lệnh **git fetch pb** để lấy thông tin đó về. 
```
$ git fetch pb
remote: Counting objects: 43, done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 43 (delta 10), reused 31 (delta 5)
Unpacking objects: 100% (43/43), done.
From https://github.com/paulboone/ticgit
* [new branch]
 master
 -> pb/master
* [new branch]
 ticgit
 -> pb/ticgit
```
Bây giờ **branch** master của **remote repository** mà tên riêng pb đại diện đã được tải về local của bạn ở vị trí **pb/master** , bạn có thể gộp branch này với môt branch khác của bạn, hoặc bạn có thể thực hiện checkout branch này.

**Fetching and Pulling from Your Remotes**
Như bạn vừa thấy, để lấy dữ liệu từ một remote repository, bạn cần thực hiện câu lệnh:
```bash
$ git fetch [remote-name]
```
Câu lệnh này sẽ lấy về mọi dữ liệu bạn chưa có ở local trong remote repository được chỉ định. Sau khi thực hiện xong, bạn thường có tất cả mọi branch của remote repository đó, những branch này bạn có thể thực hiện **merge** hoặc **inspect** chúng bất cứ khi nào. 
Nếu bạn thực hiện **clone** một repository, câu lệnh **git clone** sẽ tự động add remote repository dưới cái tên **origin**. **Git fetch origin** lấy về mọi thay đổi đã được tạo ra trên server tính từ khi bạn clone remote repository về. Có một chú ý quan trọng là **git fetch** chỉ lấy về dữ liệu mới nhất, chứ không thực hiện thao tác merge dữ liệu từ **remote repository** với dữ liệu hiện tại đang có trong **working directory** ở local của bạn. Bạn phải tự thực hiện thao tác **merge** bằng tay sau khi câu lệnh git fetch lấy dữ liệu về. 

Nếu branch hiện tại - **current branch** bạn đang làm việc có follow một brach khác trên một **remote repository** (chương 3 sẽ đề cập tới các nội dung liên quan đến **branch** và thao tác **local branch** tracking **remote branch**), thì bạn có thể sử dụng câu lệnh **git pull** để tự động làm cả 2 thao tác **fetch** dữ liệu từ **remote repository** rồi sau đó **merge** dữ liệu nhận được từ remote với dữ liệu đang có trong **local repository**. Điều này sẽ giúp bạn sử dụng **git** một cách dễ đàng và thoải mái hơn, và  (**!important**) mặc định thì sau khi câu lệnh **git clone** hoàn tất, **git** tự động thiết lập rằng **branch master** trong **local repository** sẽ **tracking** **master branch** trên **remote repository** nằm trên server mà bạn clone repository về. Khi chúng ta chạy câu lệnh **git pull** (không truyền thêm bất kỳ **option** nào vào sau câu lệnh) về cơ bản là:  **(1)** git sẽ lấy dữ liệu mới trên **remote server** chứa **remote repository** mà chúng ta sử dụng để clone về, sau đó **(2)** git sẽ cố gắng tự động **merge** dữ liệu mới lấy được với dữ liệu hiện tại đang có trong **working directory** của bạn. 

**Pushing to Your Remotes**
Khi **local repository** của bạn đạt đến một mốc mà bạn muốn chia sẻ với những người khác, bạn cần push dữ liệu của bạn lên remote repository. Câu lệnh thực hiện thao tác này là **git push [remote-name] [branch-name]** , ví dụ bạn có thể thực hiện việc up các commit tạo ra ở local lên **branch master** trên **remote repository** có tên là origin bằng câu lệnh:

```bash
$ git push origin master
```
Câu lệnh này sẽ chỉ làm việc bình thường nếu như bạn có quyền **write access** lên **remote repository** và không ai làm việc cùng remote repository thực hiện việc push commit lên kể từ commit cuối cùng được đồng bộ giữa **local repository** và **remote repository**. Nếu như một ai đó cùng làm việc cùng **remote repository** với bạn và thực hiện việc push một vài commit lên trước khi bạn thực hiện việc push các commit của bạn, thì thao tác push của bạn sẽ bị từ chối. Bạn cần phải thực hiện  việc **fetch** các commit của người kia về và thực hiện việc **merge** các nội dung trong các commit đó với nội dung của bạn, sau đó bạn mới có thể thực hiện thao tác **push**.  

**Inspecting a Remote**
Nếu bạn muốn kiểm tra thông tin chi tiết của một remote repository, bạn có thể sử dụng câu lệnh **git remote show [remote_name]**. Ví dụ:
```bash
$ git remote show origin
* remote origin
Fetch URL: https://github.com/schacon/ticgit
Push URL: https://github.com/schacon/ticgit
HEAD branch: master
Remote branches:
master
 tracked
dev-branch
 tracked
Local branch configured for 'git pull':
master merges with remote master
Local ref configured for 'git push':
master pushes to master (up to date)
```
Có thể thấy câu lệnh này trả về kết qủa là URL tương ứng với **remote repository**, cũng như các thông tin về branch, và câu lệnh này cũng cho bạn biết bạn đang ở **branch master**, và nếu bạn thực hiện câu lệnh **git pull**, git sẽ tự động thực hiện việc **merge** master branch trên remote repository với master branch trên local repository. 

Ví dụ vừa rồi là một ví dụ mà bạn có thể bắt gặp thường xuyên. Khi bạn sử dụng **git** nhiều hơn, thực hiện nhiều dự án phức tạp hơn, bạn có thể thấy thông tin về remote repository có dạng như sau:
```bash
$ git remote show origin
* remote origin
URL: https://github.com/my-org/complex-project
Fetch URL: https://github.com/my-org/complex-project
Push URL: https://github.com/my-org/complex-project
HEAD branch: master
Remote branches:
master
 tracked
dev-branch
 tracked
markdown-strip
 tracked
issue-43
 new (next fetch will store in remotes/origin)
issue-45
 new (next fetch will store in remotes/origin)
refs/remotes/origin/issue-11
 stale (use 'git remote prune' to remove)
Local branches configured for 'git pull':
dev-branch merges with remote dev-branch
master
 merges with remote master
Local refs configured for 'git push':
dev-branch
 pushes to dev-branch
 (up to date)
markdown-strip
 pushes to markdown-strip
 (up to date)
master
 pushes to master
 (up to date)
``` 
kết qủa ta nhận được chỉ ra branch nào sẽ được tự động push dữ liệu lên khi bạn chạy câu lệnh **git push**, những branch nào trên remote server mà chưa có trên **local repository**, những **remote branches** nào đã bị bạn xóa khỏi **remote server**, và nhữn local branches nào có thể được tự động **merge** với các **remote-tracking branch** khi bạn thực hiện câu lệnh **git pull**.

Để đổi tên branch, thực hiện câu lệnh **git remote rename**
```bash
$ git remote rename pb paul
$ git remote
origin
paul
```
Nếu bạn muốn loại bỏ một remote vì một vài lý do, sử dụng câu lệnh **git remote rm**
```bash
$ git remote rm paul
$ git remote
origin
```


##GitHub Pull requests
Pull request là một chức năng cho phép 

##git status command
*git status* là câu lệnh hiển thị các sự khác biệt giữa trạng thái hiện tại của repository ở folder hiện tại so với commit cuối cùng trên repository này.  Các sự khác biệt ở đây là:
- Các file mới thêm vào chưa từng xuất hiện trong repo (untracked file)
- Các file bị thay đổi nội dung so với commit cuối cùng(modified file)
- Các file sẽ được thêm vào trong commit tiếp theo (new file)
- Các file đã bị xóa đi so với commit cuối cùng
....
##git log command
git log hiển thị  danh sách các commit và thông tin của từng commit có trong repository.

## git branch tutorial
[Tutorial by english](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)


Chúng ta sẽ xem xét 1 ví dụ về branching and merging với 1 workflow mà bạn có thể gặp trong thực tế. Gỉa sử công việc của bạn đang bao gồm những công việc sau:
1. Tạo ra 1 website
2. Tạo git repository và tạo ra nhánh master cho repository đó
3. Thực hiện lập trình trên branch master
Sau bước này, bạn có thể gặp một số vấn đề và bạn cần tạo ra 1 bản vá cho vấn đề đó. Bạn sẽ phải thực hiện các công việc sau:
1. Đi tới nhánh mà bạn đang lập trình.
2. Tạo 1 nhánh mới để thêm bản vá vào cho lỗi gặp phải
3. Sau khi bản vá đã được kiểm tra, hợp nhất bản vá vào branch, sau đó push nó lên branch master.
4. Chuyển về nhánh master và tiếp tục công việc của bạn.
Đầu tiên, branch master của bạn có thể có dạng như là một chuỗi các commit:
![Ví dụ về các commit đang có trong branch master](https://git-scm.com/book/en/v2/images/basic-branching-1.png)

Bạn quyết định rằng bạn sẽ tạo ra bản vá cho issue  #53. Để tạo bản vá này, bạn cần tạo ra branch mới và thực hiện công việc vá lỗi trên branch này.  Để tạo ra 1 branch mới từ branch hiện tại, bạn có thể sử dụng câu lệnh `git checkout` để vừa tạo 1 branch mới vừa chuyển qua luôn branch mới này:
```bash
$ git checkout -b iss53
Switched to a new branch "iss53"
```
Câu lệnh phía trên là tổng hợp của 2 câu lệnh sau:
```
$ git branch iss53
$ git checkout iss53
```
Sau bước này, bạn đã tạo ra 1 branch mới có tên là **iss53** đồng thời bạn cũng đã chuyển qua branch mới này.
![Ví dụ về tạo và chuyển qua branch mới](https://git-scm.com/book/en/v2/images/basic-branching-2.png)
Giải thích ý nghĩa của hình vẽ:
Ở hình vẽ này, bạn nhìn thấy branch master và iss53 đều trỏ vào commit C2. Điều này có nghĩa là, trong repo hiện tại đang có 3 commit C0, C1, C2, trong đó C0 là commit đầu tiên, và C2 là commit hiện tại chúng ta đang xử lý (Commit mới nhất). 

Quay trở lại.

Bạn đã switch qua branch **iss53**, và  khi bạn tiến hành chỉnh sửa và commit, các chỉnh sửa và commit đó sẽ được đặt lên branch **iss53**, vì khi bạn checkout qua branch này, HEAD của bạn sẽ trỏ tới branch này.
```
$ vim index.html
$ git commit -a -m 'added a new footer [issue 53]'
```
![do some work in iss53](https://git-scm.com/book/en/v2/images/basic-branching-3.png)
Lúc này chúng ta có thể nhìn thấy, branch **iss53** đã có thêm commit C3 sau khi bạn thực hiện câu lệnh commit.

Bây giờ, có một tình huống mới xảy ra, đó là có một lỗi mới xuất hiện (**iss54**) trên nhánh master của bạn, và bạn phải lập tức sửa nó. Với Git, bạn không cần phải cùng 1 lúc vừa sửa issue 53 vừa sửa lỗi mới trên cùng 1 branch. Tất cả những gì bạn cần làm là commit những gì bạn đang làm lên branch **iss53** và trở lại branch master.
Tuy nhiên, trước khi bạn làm điều này, bạn cần lưu ý rằng nếu bạn chưa lưu lại các thay đổi của bạn so với commit cuối cùng trên branch **iss53**, Git sẽ không cho phép bạn trở lại branch **master**. Điều bạn cần làm là clean lại working state của branch **iss53** bằng cách commit các thay đổi, trước khi chuyển về branch **master**.

Gỉa sử rằng bạn đã commit tất cả các thay đổi của bạn trên **iss53**, bây giờ bạn có thể thực hiện việc chuyển về branch **master** bằng cách thực hiện câu lệnh:
```
$ git checkout master
Switched to branch 'master'
```
Bây giờ, bạn đã chuyển về branch **master**, tại thời điểm này, project working directory của bạn chính xác ở tình trạng mà trước khi bạn bắt đầu làm việc với branch **iss53**, và bây giờ bạn có thể thực hiện việc sửa 
**iss54** trên branch **master**. Một điểm quan trọng mà bạn phải ghi nhớ, đó là khi bạn chuyển về branch  **master**, Git sẽ reset working directory của bạn về đung trạng thái mà lần cuối bạn commit vào branch đó. Bằng cách thêm, xóa và thay đổi các files, Git đảm bảo rằng mọi thứ trong working directory của bạn sẽ giống y hệt như lần cuối bạn commit vào branch mà bạn sẽ chuyển qua.

Tiếp tục, chúng ta sẽ tạo ra 1 branch mới từ commit cuối cùng của branch **master** để sửa **issue54**. Sau đó thực hiện một số thay đổi để fix **isssue54** này
```
$ git checkout -b hotfix
Switched to a new branch 'hotfix'
$ vim index.html
$ git commit -a -m 'fixed the broken email address'
[hotfix 1fb7853] fixed the broken email address
 1 file changed, 2 insertions(+)
```
![tạo ra hoxfix branch để sửa issue 54](https://git-scm.com/book/en/v2/images/basic-branching-4.png)
Bạn có thể thực hiện các kiểm tra của bạn, đảm bảo rằng bản vá cho **issue54** hoạt động như ý bạn muốn, sau đó bạn có thể gộp bản vá này vào brach **master** để có thể triển khai. Bạn có thể gộp branch **hotfix** mới tạo lúc trước vào branch **master** bằng cách chuyển về branch **master** rồi thực hiện câu lệnh **git merge**
```
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
```
Sau khi thực hiện câu lệnh trên, commit **C4** mà bạn tạo ra ở hotfix branch được sao chép vào master branch, đơn giản bằng trỏ HEAD của branch **master**vào  commit **C4**  :
![git merge branch](https://git-scm.com/book/en/v2/images/basic-branching-5.png)
Sau bước này chúng ta đã thêm được các thay đổi trên branch hotfix vào branch master, lúc này chúng ta không cần tới branch hotfix nữa và có thể xóa nó đi (như bạn có thể thấy, sau khi thực hiện câu lệnh merge thì master và hotfix đều có HEAD chỉ vào commit C4). Thực hiện xóa branch hotfix bằng câu lệnh:
```
$ git branch -d hotfix
Deleted branch hotfix (3a0874c).
```
Sau khi fix xong **issue54** và deploy bản vá issue này lên branch master bằng các bước phía trên, bây giờ chúng ta có thể quay lại branch **iss53** để tiếp tục fix **issue53**. Chúng ta thực  hiện các câu lệnh sau
```
$ git checkout iss53
Switched to branch "iss53"
$ vim index.html
$ git commit -a -m 'finished the new footer [issue 53]'
[iss53 ad82d7a] finished the new footer [issue 53]
1 file changed, 1 insertion(+)
```
Chúng ta vừa thực hiện một số câu lệnh giúp chúng ta chuyển về branch **iss53**, sau đó thực hiện một số thay đổi và commit thêm 1 commit lên branch này, kết qủa thu được như sau:
![enter image description here](https://git-scm.com/book/en/v2/images/basic-branching-6.png)
Commit **C5**được thêm vào branch **iss53**.
Sau đây, chúng ta sẽ thực hiện 1 phép gộp nhánh đơn gỉan.

Gỉa sử rằng chúng ta đã chỉnh sửa xong **issue53**. Lúc này chúng ta cần gộp những thay đổi trên branch **iss53** vào branch **master**. Qúa trình này khá giống với qúa trình chúng ta gộp branch hotfix vào branch master lúc trước.  
```
$ git checkout master
Switched to branch 'master'
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
```
Trong trường hợp này, việc merge branch **iss53** vào branch **master** sẽ có chút khác biệt so với việc merge branch **hotfix** vào branch **master**. Lý do là bởi vì commit mới nhất trên branch **master** là commit **C4** không còn là tổ tiên trực tiếp của branch **iss53** như trong trường hợp trước nữa. Do vậy Git sẽ phải làm 1 số công việc, thực hiện việc gộp 3 bước (three-ways merge). Git sẽ phải xác định commit là tổ tiên chung của cả 2 nhánh **master** và **iss53**, sau đó tạo 2 snapshot 2 tập hợp các commit phía sau commit tổ tiên trên 2 nhánh **master** và **iss53**.
![enter image description here](https://git-scm.com/book/en/v2/images/basic-merging-1.png)
Sau khi thực hiện việc tạo ra các thành phần trên, một snapshot mới được tạo ra bằng cách gộp (merge) 2 snapshot kia lại với nhau, và một commit mới được tạo ra sau để thể hiện snapshot mới này. Commit này được gọi là một merge commit, điều đặc biệt đó là vì nó là kết qủa do gộp 2 snapshot lại với nhau, nên commit này có 2 tổ tiên chứ không phải là 1 như bình thường.
![enter image description here](https://git-scm.com/book/en/v2/images/basic-merging-2.png)
Một điều đáng nói ở đây, đó là việc git sẽ tự động tìm ra commit chung nhất giữa 2 branch để thực hiện việc gộp 2 nhánh, việc này làm cho Git trở nên dễ sử dụng hơn so với các hệ thống khác.
Nếu không xảy ra xung đột, git sẽ tự động thực hiên việc merge 2 branch lại với nhau và tạo ra 1 commit mới. Sau đó HEAD của master sẽ trỏ vào commit này. Sau đó bạn có thể xóa commit **iss53** đi:
```
$ git branch -d iss53
```
**Xử lý xung đột trong merge branchs**
Thông thường thì qúa trình gộp các branch có thể diễn ra không thông suốt nếu  giữa các branch có sự xung đột với nhau. Ví dụ:
```
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```
Lúc này Git không thể tự động tạo ra merge commit cho chúng ta. Nó sẽ dừng tiến trình gộp 2 branch lại để chờ bạn xử lý xung đột giữa 2 branch. 
Bạn có thể dùng ```git status``` để kiểm tra xem các thành phần nào trong working directory đang bị xung đột.
```
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:      index.html

no changes added to commit (use "git add" and/or "git commit -a")
```
Bạn cần vào các thành phần (các file) này để tự tay sửa lỗi xung đột.

Sau khi bạn xử lý xung đột xong, bạn cần thực hiện việc add các file bị xung đột vào và tạo một commit mới. 
```
git commit
```
sau khi câu lệnh này được thực hiện, merge commit được tạo ra, qúa trình gộp 2 branch với nhau hoàn tất.
> Written with [StackEdit](https://stackedit.io/).