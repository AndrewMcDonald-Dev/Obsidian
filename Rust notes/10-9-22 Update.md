# Day 1 learning Rust

Got the compiler installed with cargo 

Cargo is a packager for rust that is used pretty much universally across the rust eco-system

## Important commands

```bash
cargo build
```
This command is used to get a build file
Add add `--release` flag to get optimized build 

```
cargo run
```
This compiles the code and runs it.
This is the way of running code in a development environment

```
cargo check
```
Doesn't fully compile code simply checks if it will compile without outputting the code
This is useful for checking for syntax errors and if usually used extensively while making a program.

## First Program Number Guesser

#### Full program
```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {guess}");

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```



#### Starting Program
```rust
use std::io;

fn main() {
    println!("Guess the number!");

    println!("Please enter your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}")

}
```
With Rust there are a set of items that are brought into scope on every program. These items are called the *prelude*. Anything not in the *prelude* that is desired has to be brought in via a use keyword. std stands for the Standard Library which we pull the io (input/output) library from. 

fn defines that main is a function. The main function is always the first thing ran by the computer. 

If println!() were written like this println() then if would be a function call but instead here the ! makes it into a macro call. No idea what this is but it says that making the note on the difference is good for later.

```rust
let mut guess = String::new();
```
A lot happens in this line. let inilializes a variable but by default variables are non-mutable so you have to add mut to change it to mutable. The function that is executed is new() on the String type. new() is the general function that most types use to create an instance of themselves.

Following that logic, io::stdin() should be pretty understandable. The function stdin() is called for the io library then .read_line(&mut guess) function is run in a functional programming style attach to the previous statement. This function reads in the input from the console and appends the data to the passed reference. 

The & marks the passed *variable* as a *reference* and *references*, just like *varaibles*, are non-mutable by default so the mut sets the passed *reference* guess as mutable. If either the & or mut weren't there a error would occur.

Next, the .expect() is a function that expects a possible error where the user does not enter anything into the input. If this code were not there a warning would appear in the console upon compile time like:
```bash
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
warning: unused `Result` that must be used
  --> src/main.rs:10:5
   |
10 |     io::stdin().read_line(&mut guess);
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = note: `#[warn(unused_must_use)]` on by default
   = note: this `Result` may be an `Err` variant, which should be handled

warning: `guessing_game` (bin "guessing_game") generated 1 warning
    Finished dev [unoptimized + debuginfo] target(s) in 0.59s
```

Finally, the line:
```rust
println!("You guessed: {guess}")
```
uses the {} string formattter that places the *guess* value directly into the string.

After testing this first part the result is outputted.
```shell
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 6.44s
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
6
You guessed: 6
```


#### Generating A Secret Number

To use the *rand* library we have to list it in the dependencies becauses it is a crate.
```toml
rand = "0.8.3"
```
When you add a new dependency you have you use *cargo build* in order to grab the crate.
```shell
cargo update
```
This command updates all of the dependencies.

#### Generating a Random Number
```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    println!("The secret number is: {secret_number}");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}");
}
```

First, this new edition of the code brings in the Rng library from the rand library whose crate we just brought in.

The next new feature is calling this new library with:
```rust
let secret_number = rand::thread_rng().gen_range(1..=100);
```
We bind secret_number to the result of the right side.
That right side starts with calling the function *rand::thread_rng()* which creates a random number generator that is then used in a functional style with *.gen_range(1..=100);*.
The gen_range function uses takes a range expression and generates a random number in that range.

The general form for this range expression is **start..=end**.

The last new line simply prints the secret_number.


#### Comparing the Guess to the Secret Number

The new code is:

```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    // --snip--
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    println!("The secret number is: {secret_number}");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}");

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```

This code results in this error:
```bash
$ cargo build
   Compiling libc v0.2.86
   Compiling getrandom v0.2.2
   Compiling cfg-if v1.0.0
   Compiling ppv-lite86 v0.2.10
   Compiling rand_core v0.6.2
   Compiling rand_chacha v0.3.0
   Compiling rand v0.8.3
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
error[E0308]: mismatched types
  --> src/main.rs:22:21
   |
22 |     match guess.cmp(&secret_number) {
   |                     ^^^^^^^^^^^^^^ expected struct `String`, found integer
   |
   = note: expected reference `&String`
              found reference `&{integer}`

For more information about this error, try `rustc --explain E0308`.
error: could not compile `guessing_game` due to previous error
```

I will get back to this error but first on to what the new code means.

First things first have to bring in the Ordering type which is an enum with another use keywork.
```rust
use std::cmp::Ordering;
```

This enum has three values Less, Greater, and Equal. These are the three outcomes that are possible when comparing two values.

Next, we add five new lines:
```rust
match guess.cmp(&secret_number) { 
	Ordering::Less => println!("Too small!"),
	Ordering::Greater => println!("Too big!"),
	Ordering::Equal => println!("You win!"),
}
```

The cmp method compares two values and can be called on anything that can be compared. It takes a reference to whatever you want to compare with. When, in this example, it compares the guess to the secret_number it returns a variant of the Ordering enum. We then use a match expression to find which variant of the Ordering enum was returned and print a string accordingly. 

A match expression is made up of *arms*. An arm consists of a pattern to match against, and the code that should be run if the value given to match fits that arm's pattern. Rust that takes the value given to match and finds which arm's pattern is true. Patterns and the match construct are powerful Rust features that let you handle many situatoins your code might run into.

Now back to the error. The error says that there are mismatched types which shows Rust's strong static type system. Though, it also has type inference like when we defined the guess variable Rust guessed that it was a string. To fix this error we add this code:

```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    println!("The secret number is: {secret_number}");

    println!("Please input your guess.");

    // --snip--

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    let guess: u32 = guess.trim().parse().expect("Please type a number!");

    println!("You guessed: {guess}");

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```

The new line is:
```rust
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

An interesting thing we will learn more about is that Rust allows us to recreate a variable which is called shadowing and if helpful for keeping code clean. We bind the new variable to *guess.trim().parse()* note that the guess in this expression refers to the original guess. The trim method on string eliminates any whitespace at the beginning and end, which is necessary as this will remove the newline character aswell. The Parse method converts a string to another type. We tell Rust explicitly what variable that it by marking guess with : u32. 

Running this program outputs:
```bash
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.43s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 58
Please input your guess.
  76
You guessed: 76
Too big!
```

#### Allowing Mulitple Guesses with Looping

The loop keywork creates an infinite loop and we will use it here to allow the user to guess as much as needed.
```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    // --snip--

    println!("The secret number is: {secret_number}");

    loop {
        println!("Please input your guess.");

        // --snip--


        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = guess.trim().parse().expect("Please type a number!");

        println!("You guessed: {guess}");

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => println!("You win!"),
        }
    }
}
```

The user can use ctrl+c to exit the loop but also can crash the program by using a non-number answer. Example:
```bash
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 1.50s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 59
Please input your guess.
45
You guessed: 45
Too small!
Please input your guess.
60
You guessed: 60
Too big!
Please input your guess.
59
You guessed: 59
You win!
Please input your guess.
quit
thread 'main' panicked at 'Please type a number!: ParseIntError { kind: InvalidDigit }', src/main.rs:28:47
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

This is suboptimal to say the least ; we want this game to also stop when the correct number is guessed.

#### Quitting After a Correct Guess

```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    println!("The secret number is: {secret_number}");

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = guess.trim().parse().expect("Please type a number!");

        println!("You guessed: {guess}");

        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

By adding the break; after the user wins the program will automatically quit out of the loop.

#### Handling Invalid Input

Now we need to stop the program from crashing when the user enters a non-number. To do this we will ignore a non-number and let the user continue guessing.

```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    println!("The secret number is: {secret_number}");

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        // --snip--

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {guess}");

        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

We switch from an expect call to a match expression to move from crashing on an error to handling the error.  Parse returns a Result type which is an enum that has the variants On and Err. 

If parse is able so successfully turn the string into a number, it will return an ok value that contains the resulting number which we then pass to be assigned to guess.

If parse is not able to turn the string into a number, it will return an Err value that contains more information about the error. The _ is a catchall value; in this example, we're saying we want to match all Err  values, no matter the info they have inside them. Continue will move the program to the next iteration of the loop. 

```bash
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 4.45s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 61
Please input your guess.
10
You guessed: 10
Too small!
Please input your guess.
99
You guessed: 99
Too big!
Please input your guess.
foo
Please input your guess.
61
You guessed: 61
You win!
```

Our program is basically complete all we have to do is remove the line that states the secret_number and build the release version.