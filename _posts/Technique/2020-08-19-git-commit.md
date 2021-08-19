---
layout: post
title: Làm thế nào để viết một Git Commit đẹp
category: Technique
---
---

# Làm thế nào để viết một Git Commit đẹp

<p>Đây là bài dịch lại từ blog của Chris Beams nói về cách viết Git Commit Message, bản gốc tiếng Anh:  <a href="https://chris.beams.io/posts/git-commit/">https://chris.beams.io/posts/git-commit/</a> </p>



<h2>Giới thiệu: Tại sao cần commit đẹp</h2>



<p>Nếu duyệt qua log của một Git repository bất kì, bạn có thể sẽ thấy các commit message với đủ kiểu dài ngắn khác nhau. Ví dụ, đây là <a rel="noreferrer noopener" aria-label="những commit đầu tiên (mở trong tab mới)" href="https://github.com/spring-projects/spring-framework/commits/e5f4b49?author=cbeams" target="_blank">những commit đầu tiên</a> vào dự án Spring của C. Beams </p>



<code>$ git log --oneline -5 --author cbeams --before "Fri Mar 26 2009"

e5f4b49 Re-adding ConfigurationPostProcessorTests after its brief removal in r814. @Ignore-ing the testCglibClassesAreLoadedJustInTimeForEnhancement() method as it turns out this was one of the culprits in the recent build breakage. The classloader hacking causes subtle downstream effects, breaking unrelated tests. The test method is still useful, but should only be run on a manual basis to ensure CGLIB is not prematurely classloaded, and should not be run as part of the automated build.
2db0f12 fixed two build-breaking issues: + reverted ClassMetadataReadingVisitor to revision 794 + eliminated ConfigurationPostProcessorTests until further investigation determines why it causes downstream tests to fail (such as the seemingly unrelated ClassPathXmlApplicationContextTests)
147709f Tweaks to package-info.java files
22b25e0 Consolidated Util and MutableAnnotationUtils classes into existing AsmUtils
7f96f57 polishing</code>

<p>Còn đây là <a rel="noreferrer noopener" aria-label="các commit khác (mở trong tab mới)" href="https://github.com/spring-projects/spring-framework/commits/5ba3db?author=philwebb" target="_blank">các commit khác</a> thuộc cùng repository </p>



<code>$ git log --oneline -5 --author pwebb --before "Sat Aug 30 2014"

5ba3db6 Fix failing CompositePropertySourceTests
84564a0 Rework @PropertySource early parsing logic
e142fd1 Add tests for ImportSelector meta-data
887815f Update docbook dependency and generate epub
ac8326d Polish mockito usage</code>



<p><strong>Bạn thích cái nào hơn?</strong><br>Cái trên dài ngắn lung tung, là cách viết thường thấy của đa số anh em; cái dưới gọn gàng và nhất quán nhưng không phải ngẫu nhiên mà họ viết như thế.<br>Hầu như các  repositories’ log đều trông như cái bên trên, nhưng vẫn có ngoại lệ. Ví dụ như&nbsp;<a href="https://github.com/torvalds/linux/commits/master">Linux kernel</a>&nbsp;và <a href="https://github.com/git/git/commits/master">chính dự án Git</a>. Thử nhìn vào <a href="https://github.com/spring-projects/spring-boot/commits/master">Spring Boot</a> hoặc bất kì repository&nbsp;nào do <a href="https://github.com/tpope/vim-pathogen/commits/master">Tim Pope</a> quản lý. Contributors của những repos này hiểu rằng viết một commit message &#8220;đẹp&#8221; là cách tốt nhất để thể hiện sự thay đổi trong mã nguồn với đồng nghiệp (và với chính bản thân contributor về sau). Một diff sẽ cho bạn biết <em>cái gì</em> đã thay đổi, nhưng chỉ có commit massage mới có thể cho biết <em>tại sao</em> có thay đổi đó.  Peter Hutterer (không biết ông này là ai)&nbsp;<a href="http://who-t.blogspot.co.at/2009/12/on-commit-messages.html">cho rằng</a>: </p>



<blockquote class="wp-block-quote"><p> Re-establishing the context of a piece of code is wasteful. We can’t avoid it completely, so our efforts should go to&nbsp;<a href="http://www.osnews.com/story/19266/WTFs_m">reducing it</a>&nbsp;[as much] as possible. Commit messages can do exactly that and as a result,&nbsp;<em>a commit message shows whether a developer is a good collaborator</em>. </p><p>Bỏ qua đoạn đầu, chỉ cần quan tâm: <em>commit message cho biết một developer có phải là một collaborator giỏi hay không.</em></p></blockquote>



<p>Nếu bạn chưa từng nghĩ đến việc tạo ra một commit đẹp có thể là vì bạn chưa dành nhiều thời gian dùng lệnh <em>git log</em> hay các công cụ tương tự. Luẩn quẩn là ở chỗ: Khi commit history không có cấu trúc và thiếu nhất quán, người ta sẽ không bỏ thời gian quan tâm đến nó; Và vì không quan tâm nên nó vẫn cứ phi cấu trúc và thiếu nhất quán.<br>Nhưng nếu nếu được quan tâm đúng mực thì log sẽ trở nên đẹp đẽ và rất hữu dụng. Những câu lệnh <em>git blame, revert, rebase, log, shortlog</em>&#8230; sẽ trở nên thú vị. Khi đó, việc duyệt commit và pull request của người khác sẽ rất có giá trị và có thể thực hiện độc lập. Việc hiểu được lí do của những thứ đã xảy ra từ vài tháng hoặc vài năm trước không chỉ dễ dàng mà còn hiệu quả.<br>Khi một dự án dài hơi kết thúc thành công và chuyển qua giai đoạn bảo trì thì đối với  maintainer, không có gì đáng giá hơn log, log mà lởm khởm thì developer ăn chửi! Nên làm code thì hãy quan tâm đến cảm xúc của thằng khác, việc này có thể gây rắc rối và mất thời gian. Nhưng khi đã thành thói quen thì sẽ giúp cải thiện năng suất cho cả những người liên quan.</p>



<p>Trong bài này, người viết chỉ đề cập đến yếu tố cơ bản nhất của việc giữ cho commit history sạch sẽ: Làm sao để viết commit message cá nhân. Còn có những thứ quan trọng khác như commit squashing nhưng không được đề cập ở đây. Tác giả nói có thể sẽ viết trong một bài khác.</p>



<p>Hầu hết các ngôn ngữ lập trình đều có những quy ước nhất định để tạo nên phong cách của riêng nó như cách đặt tên, định dạng mã nguồn&#8230; Tất nhiên các qui tắc cũng có nhiều biến thể khác nhau, nhưng hầu như các developer đều đồng ý rằng chọn và gắn bó với một thứ sẽ tốt hơn để mỗi người là một kiểu, như vậy sẽ rất hỗn loạn.<br>Tiếp cận commit log của một team cũng không khác biệt gì. Để tạo ra được revision history &#8220;tử tế&#8221;, cả team cần phải thống nhất qui ước về nội dung commit (commit message convention), qui ước này ít nhất phải xác định được 3 điều sau:</p>



<ul><li><strong>Style. </strong>Cú pháp, canh lề, ngữ pháp, cách viết hoa, chấm câu. Làm sao để mọi thứ trở nên đơn giản nhất có thể, để tạo ra dòng log dễ đọc dễ hiểu có thể xem xét ở mọi lúc mọi nơi.</li><li><strong>Content.</strong> Loại thông tin nào nên được đưa vào phần thân của commit message, loại nào không?</li><li><strong>Metadata.</strong> Làm sao để quản lí và tham chiếu tracking ID, pull request number&#8230;?</li></ul>



<p>Hên là những qui ước này đã có sẵn đầy ra rồi, không có gì cần làm lại cả. Chỉ cần theo 7 qui tắc dưới đây, bạn sẽ commit như một chuyên gia.</p>



<h2>Bảy qui tắc của một Git commit đẹp</h2>



<ol><li>Tách Tiêu đề khỏi phần Nội dung bằng một dòng trống</li><li>Giới hạn dòng Tiêu đề không quá 50 kí tự</li><li>Viết hoa chữ cái đầu Tiêu đề</li><li>Không kết thúc Tiêu đề bằng dấu chấm</li><li>Sử dụng câu mang tính bắt buộc trong Tiêu đề</li><li>Gói mỗi dòng của Nội dung trong 72 kí tự</li><li>Phần nội dung sẽ giải thích: Cái gì, Tại sao, Làm thế nào?</li></ol>



<p>Ví dụ</p>



<code>Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequences of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789</code>



<h3>1. Tách Tiêu đề khỏi phần Nội dung bằng một dòng trống</h3>



<p>Trích từ <em>git commit</em> manpage:</p>



<blockquote class="wp-block-quote"><p>Though not required, it’s a good idea to begin the commit message with a single short (less than 50 character) line summarizing the change, followed by a blank line and then a more thorough description. The text up to the first blank line in a commit message is treated as the commit title, and that title is used throughout Git. For example, Git-format-patch(1) turns a commit into email, and it uses the title on the Subject line and the rest of the commit in the body.</p></blockquote>
	</div>