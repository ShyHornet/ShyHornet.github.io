---
layout: post
title:  "欢迎访问我的博客!"
categories: jekyll update
---

You'll find this post in your `_posts` directory - edit this post and re-build (or run with the `-w` switch) to see your changes!
To add new posts, simply add a file in the `_posts` directory that follows the convention: YYYY-MM-DD-name-of-post.ext.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}
{% highlight swift %}
func login(){
        let username = self.username.text
        let password = self.password.text

        guard username?.getLength() >= 4 else{
            SVProgressHUD.showErrorWithStatus("用户名长度必须大于4个字符")
            return
        }
        guard password?.getLength() >= 8 else{
           SVProgressHUD.showErrorWithStatus("密码长度必须大于8个字符")
            return
        }



        PFUser.logInWithUsernameInBackground(username!, password: password!)
        { (user,error) -> Void in

            guard user != nil else{
            print(error?.userInfo)


              SVProgressHUD.showErrorWithStatus("登陆失败╮(╯﹏╰）╭")

                return
            }
            SVProgressHUD.dismiss()
            dispatch_async(dispatch_get_main_queue(), { () -> Void in


                self.navigationController?.popViewControllerAnimated(true)
            })


        }
    }

{% endhighlight %}
Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll's GitHub repo][jekyll-gh].

[jekyll-gh]: https://github.com/jekyll/jekyll
[jekyll]:    http://jekyllrb.com
