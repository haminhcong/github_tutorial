
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
Bây giờ, bạn cần tập trung chú ý, đây là một điều quan trọng về Git mà bạn cần nhớ nếu bạn muốn từ bây giờ qúa trình đọc về Git trở nên suôn sẻ và trôi chảy. Trong git, một file/folder tồn tại ở 1 trong 3 trạng thái chính: committed, modified, and staged. Committed nghĩa là dữ liệu về file đó đã được lưu trữ an toàn trong local database. Modified có nghĩa là bạn đã thay đổi nội dung của file nhưng chưa commit nó vào local database đồng thời cũng  **chưa được đánh dấu là đã được stagged**. Staged có nghĩa là bạn đã đánh dấu một file bị thay đổi sẽ lưu trạng thái hiện tại của nó vào snapshot ở lần commit tiếp theo.
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
```
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