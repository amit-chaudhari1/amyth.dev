---
title: "Write Code for Humans, Not Machines"
date: 2020-09-15T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["draft"]
author: "Amit Chaudhari"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: true
draft: true
hidemeta: false
comments: false
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "coverImages/xkcd_for_humans_not_machines.png" # image path/url
    alt: "<alt text>" # alt text
    caption: "" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---
Any fool can write code that a computer can understand. Good programmers write code that humans can understand.

## **Inspiration**

*Any fool can write code that a computer can understand. Good programmers write code that humans can understand.Martin Fowler, 2008.*

*Always code as if the guy who ends up maintaining your code will be a violent psychopath who knows where you liveJohn F. Woods, 1991*

## **Introduction**

Everyone is a beginner at some point. If you learned to code in the university with a teacher that forced you to write good code, you have a good starting point. But sadly, most people learn on their own or with **no proper guidance in code quality**. Some people eventually realize that the code quality is important, but they **lack of the time** to do it. This is the typical situation when you work under pressure or time constrains.

It is hard to explain to you boss that you need another week to prepare your code when it is “already working”. So you ship the code anyway because you can not afford to spent one week more. One year later, a simple upgrade is required and when you boss asks about timing, you have to explain that it will take 2 weeks. Also, the chances of breaking something are really high, because not even you remember how the code actually works.

Code quality alone wont solve the previous issue, but together with a good test coverage, it can considerably reduce the pain and improve the quality of the application for both users and programmers.

## **Sources of poor quality code**

### **Dark Wizards**

To see if you are writing good code, you can question yourself. *how long it will take to fully transfer this project to another person?* If the answer is *uff, I don’t know… a few months…* your code is like a **magic scroll**. most people can run it, but no body understand how it works. Strangely, I’ve seen several places where the **IT department** consist in **dark wizards** that craft scrolls to magically do things. The less people that understand your scroll, the more powerfully it is. Just like if life were a video game.

Writing spells is fun, but it is definitely not a good idea if you intend to work ast a team. If you still do not understand what a spell is, let me show you a few examples in the language that only the darkest of the wizards can master. **bash**. I challenge you to tell me what it does without looking at the source

```bash
# http://www.bashoneliners.com/oneliners/269/
netstat -tn 2>/dev/null | grep :80 | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr | head

# http://www.bashoneliners.com/oneliners/277/
find . -print0 | xargs -0 -P 40 -n 1 sh -c 'ffmpeg -i "$1" 2>&1 | grep "Duration:" | cut -d " " -f 4 | sed "s/.$//" | tr "." ":"' - | awk -F ':' '{ sum1+=$1; sum2+=$2; sum3+=$3; sum4+=$4; if (sum4 > 100) { sum3+=1; sum4=0 }; if (sum3 > 60) { sum2+=1; sum3=0 }; if (sum2 > 60) { sum1+=1; sum2=0 } if (NR % 100 == 0) { printf "%.0f:%.0f:%.0f.%.0f\n", sum1, sum2, sum3, sum4 } } END { printf "%.0f:%.0f:%.0f.%.0f\n", sum1, sum2, sum3, sum4 }'

# http://www.bashoneliners.com/oneliners/271/
man -k . | awk '{ print $1 " " $2 }' | dmenu -i -p man | awk '{ print $2 " " $1 }' | tr -d '()' | xargs man -t | ps2pdf - - | zathura -
```

> If I were to hire a programmer. I would like to hire an actual programmer and not a wizard.
> 

### **Copy paster**

Another source of poor quality code is **Stack overflow**. Although copy paste is an essential skill for any developer, copy paste without understanding yields to code that not even you understands and side effects that will be really hard to foresee.

> The rule of thumbs is, never use code that you do not understand.
> 

I think this is better explained by an example.

Let’s imagine the situation where a developer is looking for “go upload file” and he may end [here](https://astaxie.gitbooks.io/build-web-application-with-golang/en/04.5.html). If you simply copy and paste this code into your app, you will have a working file upload server… with at least one security issue…

When you upload the file, the application uses the field “name” from the file upload to save the file to the disk appending the name to the download directory. The issue is that while the browser always sends only the file name, a request can be crafted to upload a different filename with a relative path. This will allow a caller to upload any file to potentially any directory in the server machine using relative paths. You can test this by yourself following the page tutorial and modifying the go client side in the following block to upload shell scripts wherever you want.

```bash
// old: fileWriter, err := bodyWriter.CreateFormFile("uploadfile", filename)
fileWriter, err := bodyWriter.CreateFormFile("uploadfile", "../../hack.sh")
if err != nil {
    fmt.Println("error writing to buffer")
    return err
}
```

Good thing is that you could have avoided that by reading the documentation from [Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition#Directives).

> ***filename**: Is followed by a string containing the original name of the file transmitted. **The filename is always optional and must not be used blindly by the application**: path information should be stripped, and conversion to the server file system rules should be done. This parameter provides mostly indicative information. When used in combination with Content-Disposition: attachment, it is used as the default filename for an eventual “Save As” dialogue presented to the user.*
> 

*Mozilla documentation*

YOU ALWAYS READ THE DOCUMENTATION.

### **Open source curse**

Open source is a double edge sword. It allows you to quickly add new functionalities to you codebase with little effort but, what happens if you find a bug in an library? When you work with a private company, you can call the company and request the bug to be fixed, but when you are using Open Source… there is no one to call aside from opening an issue in Github and hope that someone fixes it.

Reached this point, people usually forgets about one of the greatest advantages of Open Source. YOU can fix the issue. You can download the source code and dig deep into the code allow you to keep moving. Also, you can merge this changes back to the original repository so others doesn’t have to fix it again. win-win relationship.

Of course, not everyone has the knowledge or the time to fix a bug in a big Open Source project, so it would be wise to do no import an Open Source into your code base unless

- You can maintain the code.
- You trust that the community is big enough to maintain it for you.

Also, it doesn’t hurt to read trough the code base even if you trust the community. It will give you an idea of the quality of the code and the future that it may have.

### **Never Refactor**

Provably you already know that it is a bad idea to name a variable “aa” or to write 1000 lines function with 8 levels of indentation. The point is,things doesn’t get that dirty right the way. Usually you already have a variable “a” that makes sense but you suddenly have to add another variable “a” and you don’t have time to think a better name. The same for the 1000 lines function, all started with a simple 10 lines function that grow up day by day.

No one can write perfectly readable code without thinking it twice, and that is when refactor comes into play.

You could easily read your code again and rename your variables “a” and “aa” to something better like “p0_a” and “p1_a” and the long function can be split into smaller functions for sure.

- see the code that I've in mind
    
    ```bash
    //bad
    a := 1
    b := 2
    c := 3
    
    x := 1
    
    p0 := a*x*x+b*x+c
    
    aa := 1
    bb := 2
    cc := 3
    
    xx := 1
    
    p1 := aa*xx*xx+bb*xx+c
    
    // better
    p0_a := 1
    p0_b := 2
    p0_c := 3
    
    p0_x := 1
    
    p0 := p0_a*p0_x*p0_x+p0_b*p0_x+p0_c
    
    p1_a := 1
    p1_b := 2
    p1_c := 3
    
    p1_x := 1
    
    p1 := p1_a*p1_x*p1_x+p1_b*p1_x+p1_c
    ```
    

So the lesson here is take your time to refactor your code. If you wait too long… it will bite you.

## **The price of the quality**

From my point of view, Some people don’t invest time in code quality because they **cannot see immediate benefits** in doing so. Investing time in refactoring to achieve good quality code and improving test coverage can be seen as **preventive maintenance**. Does having a high test coverage and a good code quality guarantee that you won’t have any bug? of course not, neither doing proper maintenance to your car will prevent it from breaking, but it will definitely reduce the chances.

> Quality is an investment.
> 

There are situations where developers argue that they won’t end maintaining the code, so the code quality doesn’t matter. Just read the second quote at the begging of this post again. Chances are that the future maintainer wont be a psychopath, but if he is a nice person, you will be making his work a lot harder. That person will think that the company behind that code is not a good company and that’s really bad for future projects. In the other hand, If the code quality is high, that person will be immensely grateful and will recommend anyone your company for doing a great job. Remember, it is an investment.

Also, after a few months without reading your own code, it feels like code developed by a total stranger. so in some cases you will be the psychopath wanting to kill yourself. So **do your future self a favour and clean your code**.

Have you ever measure the cost of a bug? Of course, it depends on the kind of bug, but if the client sees it, even if it is something small, the cost is really high. You are **loosing confidence** and that is really hard to get in the first place and even harder to recover.

## **Tips for clean code**

This tips should apply to almost any programming language.

### **Do not use “else” for error checking**

Let’s start with the typical case when else is used for error handling

```
if r.Method == "GET" {
    if r.Header.Get("X-API-KEY") == key {
        // ok
        return nil
    } else {
        return errors.New("Key is not valid")
    }
} else {
    return errors.New("Invalid Method")
}
```

This code contains 2 levels of indentation and it can easily get more levels if you keep this pattern. The best way to handle it is to inverse the logic of the error checking. See the following example

```
if r.Method != "GET" {
    return errors.New("Invalid Method")
}
if r.Header.Get("X-API-KEY") != key {
    return errors.New("Key is not valid")
}
return nil
```

This code is much easier to understand as it do not add levels of indentation and follows the principle where the 0 indentation level is the principal path of the application where other paths are exceptions or rare cases.

**else** is still valuable in some cases, for example when choosing between multiple methods. But I still prefer to use a switch for this situations.

```bash
if r.Method == "GET" {
    return handleGET()
} else if r.Method == "POST" {
    return handlePOST()
} else {
    return errors.New("Invalid Method")
}
```

Keep in mind that the previous example is only readable because the body of the it blocks is only a function. If you add code inside the blocks… this gets dirty pretty quickly.

### **Comments? No. Documentation? Yes**

If you have to write comments to explain your code… it is not clean code.

When I say comments, I refer mostly to inline comments. You will find different kind of those comments so let’s start with one of the most frequent. The tutorials for beginners.

```
// Check if request method is GET
if r.Method == "GET" {
    return handleGET()
// If is not GET, check if it is POST
} else if r.Method == "POST" {
    return handlePOST()
// If is not GET or POST, return an error
} else {
    return errors.New("Invalid Method")
}
```

Do you really need those lines? Isn’t it clear enough? Well, I still find comments like this in a lot of cases and it gets funnier when the comment is obsolete and the code is doing other things. Following the example, let’s imagine that another developer adds support for **PUT** method. I think that this is what would happen.

```
// Check if request method is GET
if r.Method == "GET" {
    return handleGET()
// If is not GET, check if it is POST
} else if r.Method == "POST" {
    return handlePOST()
// Handle PUT to modify values
} else if r.Method == "PUT" {
    return handlePOST()
// If is not GET or POST, return an error
} else {
    return errors.New("Invalid Method")
}
```

Again, this is a simplified code and the error in clear. but in real world cases… this happens more often that you may think.

```
## Solves the FizzBuzz Problem
## prints the numbers 1-100, each on a new line
## For each number that is a multiple of 3, print “Fizz” instead of the number
## For each number that is a multiple of 5, print “Buzz” instead of the number
## For each number that is a multiple of both 3 and 5, print “FizzBuzz” instead of the number
(lambda f, n: f(f, n))(lambda f, n: [(not n % 3 and "fizz" or "") + (not n % 5 and "buzz" or "") or n] + f(f, n+1) if n <= 100 else [], 1)

```

---

Another type of comments are the ones trying to explain a spell. This time in Python.

> Do not explain your spells, rewrite them.
> 

This is the moment for jack.

![https://metalblueberry.github.io/post/blog/2020-06-14_write_code_for_humans_not_for_computers/but_it_runs.jpg#center](https://metalblueberry.github.io/post/blog/2020-06-14_write_code_for_humans_not_for_computers/but_it_runs.jpg#center)

Now look at the following code.

```
def process(number):
    if number % 3 == 0 and number % 5 == 0:
        return 'FizzBuzz'
    elif number % 3 == 0:
        return 'Fizz'
    elif number % 5 == 0:
        return 'Buzz'
    else:
        return number

def printFizzBuzz(n=101):
    for i inrange(1, n):
        print(process(i))
```

This is so clear that you don’t even need comments to explain it.

The last type of comment is the documentation comment. This is the only comment that is really required and you will understand why in a moment.

```
def printFizzBuzz(n=101):
    """
    The concept behind FizzBuzz is as follows:

    Write a program that prints the numbers 1-100, each on a new line
    For each number that is a multiple of 3, print “Fizz” instead of the number
    For each number that is a multiple of 5, print “Buzz” instead of the number
    For each number that is a multiple of both 3 and 5, print “FizzBuzz” instead of the number
    """
    for i inrange(1, n):
        print(process(i))
```

> Code explains what and howDocumentation explains why.
> 

So make sure to write your documentation, but do not explain your spells.

#### **Abstraction levels and cognitive capacity**

> Cognitive capacity is the total amount of information the brain is capable of retaining at any particular moment. This amount is finite, so we can say our total capacity is only ever 100%. How much of one’s cognitive capacity is being used towards a particular task at any given time is called the cognitive load
> 

This section is closely related to [else error checking](https://metalblueberry.github.io/post/blog/2020-06-14_write_code_for_humans_not_for_computers/#do-not-use-else-for-error-checking). When you have the following code, your cognitive load increases with the level of indentation.

```
if r.Method == "GET" {
    if r.Header.Get("X-API-KEY") == key {
        // ok
        return nil
    }else{
        return errors.New("Key is not valid")
    }
} else {
    return errors.New("Invalid Method")
}
```

In more complex programs it is really easy to run out of cognitive capacity and finding yourself going back and forth looking for what the code was doing.

So, knowing that the cognitive capacity is finite. You have to do your best to reduce it to the minimum. There are several tips for that.

- Write a good documentation so programmers have a good foundation of the reason behind the code
- Avoid indentation levels, If you find yourself with more than 3, you should create a function.
- Never use writeable global variables and only use read-only when there is no other option.
- Write small functions that fit into the screen.
- Do not lie in the purpose of a function.

I’m going to explain the last one.

```
func ReadConfFile(path string) (*Conf, error){
    confContent, err:= ioutil.ReadAll(path)
    if err != nil {
        return DefaultConf()
    }
    conf:= &Conf{}
    err:=json.Unmarshal(confContent, conf)
    if err != nil {
        return nil, err
    }
    return conf
}

```

---

This function is not bad. It is clear and short. But it lies in it’s purpose. When you read `ReadConfFile` you don’t expect the function to return de DefaultConf if the configuration file do not exist. This can be rewritten.

```
func ReadConfFile(path string) (*Conf, error) {
    confContent, err := ioutil.ReadAll(path)
    if err != nil {
        return nil, err
    }
    conf := &Conf{}
    err := json.Unmarshal(confContent, conf)
    if err != nil {
        return nil, err
    }
    return conf, nil
}
func DefaultConf() *Conf {
    return &Conf{}
}

func LoadConfiguration() *Conf {
    conf, err := ReadConfFile(os.Getenv("CONF_FILE"))
    if err != nil {
        return DefaultConf()
    }
    return conf
}
```

This is much easier to understand from the point of view of cognitive load. The `LoadConfiguration` function tells you that always returns a valid configuration because it can not return an error. Once you open it, you clearly see that it tries to read a configuration file based on a env variable and if fails, returns the default configuration. Now you can get deeper into those functions in case you need more information.

The rule of thumb here is. If you cannot start adding new code to your own code within 5 minutes of opening the editor, it provably requires to much cognitive capacity. If you need 5 minutes with a code that it is already familiar to you… just imagine others that have never seen that code before.

#### **Do not write an utils/common/miscellaneous package**

Seriously… Don’t do it. The purpose of creating a package is to reuse code, but this advantage comes with a huge drawback. **Dependencies**

People tend to create a common package when the use a function over and over again but they don’t know where to put it. Well, if it is a simple small function… maybe you should simply copy and paste it instead of creating a new dependency for your code. Let’s see and example.

```
// envMustBeSet panics with a proper error message if the required variable is not set.
func envMustBeSet(key string) string {
    value, ok := os.LookupEnv(key)
    if !ok {
        log.Panicf("environment variable %s must be set", value)
    }
    return value
}

```

---

As you can imagine, if your application reads configuration from the environment, this function can be really handy at startup to fail quickly if you don’t have the proper environment variables defined. but, is it worth to import it from an external module? If you need to use it from two different packages, I think it is easier to maintain two identical function in both packages than to create a third package.

> A little copy paste is better than a little dependency.
> 

To explain the dependency problem, I’ve used the log package for this implementation but I’ve not provided any way of configuring it. Adding another parameter for the log will make this function less handy. what happen if you have to redirect the output of that log and you are using logrus instead of the standard logger?. You will be forced to modify the standard logger in addition to your logrus instance. you could go to the common package and change the log package for the logrus package but then… other dependencies will be forced to import logrus package just to print this line… do you see the problem?

Well, this case is simple, but you will eventually find a functions that you may want to share between packages and is complex enough to do not want to copy/paste it. This is when the common package usually appears. Following this pattern, it quickly becomes a mess of non related functions that anyone but the owner feels brave enough to import. Reading a common package like this requires a LOT of cognitive capacity. Said that, common and miscellaneous packages are a bad idea in most cases, but the util package may have its place. For example, look at packages like `io/ioutil`. This package groups utility function to perform io operations. Going back to the `envMustBeSet` function. It may be a good idea to place it in `env/envutils` with other environment variable utilities like reading numbers from the environment.

- miscellaneous: NEVER
- common: NEVER
- util: only if you specify a scope

#### **Use blank lines to separate logical groups**

This one is really simple, yet effective. just compare this two codes

```
func main() {
    f, err := os.Create("test.txt")
    if err != nil {
        fmt.Println(err)
        return
    }
    l, err := f.WriteString("Hello World")
    if err != nil {
        fmt.Println(err)
        f.Close()
        return
    }
    fmt.Println(l, "bytes written successfully")
    err = f.Close()
    if err != nil {
        fmt.Println(err)
        return
    }
}

```

---

```
func main() {

    f, err := os.Create("test.txt")
    if err != nil {
        fmt.Println(err)
        return
    }

    l, err := f.WriteString("Hello World")
    if err != nil {
        fmt.Println(err)
        f.Close()
        return
    }

    fmt.Println(l, "bytes written successfully")

    err = f.Close()
    if err != nil {
        fmt.Println(err)
        return
    }
}
```

Your brain automatically stops reading at blank lines drastically reducing the cognitive load to understand the code. Also, the brain recognises the pattern of the code and this reduces even more the load.

#### **Chose the right tool**

Even though the tips are independent from the programming language. The language still plays a role in how you follow the tips. As a general rule, refactor operations are much easier in typed languages because you can easily detect missing variables and rename operations works much better. So I would recommend to avoid using scripting languages like **javascript** or **python** for long running projects as you will end wasting most of the time maintaining the code or writing huge testing batteries to ensure everything is still working as expected.

One of the reasons why I love go is because of the tooling that allows you to ensure code quality, `gofmt` and `go vet` are awesome and easy to use. And `gopls` is improving every day.

#### **Always write tests**

I didn’t say, “write a test for every function”. but at least write tests to ensure the main functionality.

As testing deserves its own post… I won’t say anything else here.

## **Conclusion**

I think the code quality issues appear more often when developer work alone. It you add more people to the team, things will get better by pure necessity to understand others people code. Keep this in mind when you are writing code by yourself and take your time to refactor so anyone can understand it.

Internet is full of good practices for writing code I recommend you to keep reading about the subject. What you’ve read here is a good starting point, but yet a brief summary.

Thank you for your reading and feel free to leave a comment If you liked it.

Or click the clap button.

## **References**

- [Python bad fizzbuzz](https://www.quora.com/What-are-some-prime-examples-of-bad-python-code)
- [FizzBuzz](https://www.blog.pythonlibrary.org/2019/09/18/python-code-kata-fizzbuzz/#:~:text=One%20of%20the%20popular%20programming%20katas%20is%20called%20FizzBuzz.&text=The%20concept%20behind%20FizzBuzz%20is,Buzz%E2%80%9D%20instead%20of%20the%20number)