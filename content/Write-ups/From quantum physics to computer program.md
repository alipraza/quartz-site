(_Disclaimer: this article won’t actually explain quantum physics, operating systems, or software development. But you should read it anyway!_)

## The Operating System Model of Personal Computers

Windows 10, Windows 11, macOS, Ubuntu, Android, iOS — you might have heard of names like these, and you might be using one of these on a hardware device (possibly a smartphone, laptop, or desktop computer) while reading this. Ever been curious about this, though? What _are_ these? How do you even know that we can bunch them together into a single category? For all we know, “Windows 10” could be something vastly different from “Android”, especially considering we use them on completely different devices. In that case, it wouldn’t be too wise to group them together.

The ‘names’ I mentioned above are all of a concept called “Operating System” (OS): Windows 10 and Android are examples of it. An OS is something that you require to actually _use_ a modern computer, i.e., devices whose ‘brain’ only deals in 0s and 1s. That’s a bit of a bold, and maybe not entirely accurate, statement. Let’s first unpack the second part: what does it mean for something to only deal in 0s and 1s?

- This ‘brain’ in computers is called the Central Processing Unit (CPU), which deals in ‘instructions’. Instructions are sequences of 0s and 1s that all (must) obey a specific format. But 0 and 1 are only concepts, and don’t exist as something standalone in real life (i.e., _something_ can be 1 in amount, but “1” **itself** is not a separate physical entity).
- To say that a CPU ‘supports’ an instruction means that the instruction adheres to the instruction format for that CPU, and the CPU contains dedicated circuitry that was designed to be ‘activated’ when that instruction is issued to the execution units comprising that circuitry. So here, we have somehow given physical meaning to sequences of 0s and 1s. (Circuits can ‘process’ these abstract 0s and 1s if they are modelled using something that _is_ physically measurable, like potential difference!)
- By following the brain analogy, the CPU can ‘read’ any sequence of 0s and 1s, but it only **recognises** a given sequence as an “instruction” if it conforms to the instruction format _and_ is part of the CPU’s ISA.

That was a lot to take in! Feel free to give it another read, this time more slowly, and perhaps do a teeny bit of Googling to fill in the inevitable gaps in understanding.

Oh, and the most important takeaway from all that is: an “Instruction Set Architecture” (ISA) isn’t something physical in the hardware, just like instructions, the instruction format, and even 0s and 1s, as stated above.

  

## How I View Abstractions

Before I shed some more light on what an ISA is, be very clear in your understanding that A LOT of things in computing aren’t physical. Mathematics is what forms the basis of computing, and math itself is not really something that is inherently physical in its nature. If you don’t believe me, give this a read: [https://en.wikipedia.org/wiki/Presheaf_(category_theory)](https://en.wikipedia.org/wiki/Presheaf_(category_theory)) … it’s an example of a mathematical concept that has nothing to do with the physical world per se! That doesn’t mean it’s unimportant, quite the opposite. We humans intentionally abstract away things that we don’t want to deal with or think about very frequently.

For example, when you want to talk to someone, will you conceptualise it in your mind using wireless communications, Radio Frequency (RF) engineering, analog and mixed signal Integrated Circuits (ICs), and all that jazz? Probably not, and instead you will think of it along the lines of: “I want to tell something to someone, so I will establish contact with the other person somehow”, which is you subconsciously applying a model of communication.

- “something” → message
- “I” → sender
- “other person” → receiver
- “somehow” → channel

This primitive model of communication, comprising the four components on the right side of the above mappings, is **_entirely_** an abstraction, a conceptual entity (as opposed to a physical entity whose existence our six senses can testify for). You come up with this model through some thoughts, apply it to _your_ situation, and then actually **do** what you want by using physical entities are available to you (e.g., fax machine, telephone, smartphone). Still, no thoughts about all the ‘technical details’, because they are at a lower level of abstraction than what you care for when doing a task like this in your daily life.

So, coming back to the ISA, it is indeed an abstraction like most other things I’ve introduced in this “model of computation”, which insofar includes operating system, central processing unit, and some unstated stuff (e.g., controller and memory). We can use a “model of physics” to describe what happens at a very low level of abstraction in a computer, like using quantum mechanical wavefunctions to describe how the physical state of each subatomic particle evolves in space-time, but we will ultimately remain limited by our lack of understanding of reality as a whole — physics is itself merely an imperfect, constantly changing tool we try to use to explain reality. That was a bit of a long, convoluted sentence, but you can better appreciate my point if you watch this excellent video that demonstrates the point I’m making, in which the presenter empirically questions an abstraction we use everyday: [https://www.youtube.com/watch?v=DqhXsEgLMJ0](https://www.youtube.com/watch?v=DqhXsEgLMJ0)

Anyway, even if we _do_ use the (currently) most ‘accurate’ model of physics to explain all the inner workings behind what happens when we use a computer, it doesn’t help us as users practically. At the end of the day, I’m not intentionally manipulating quantum states of subatomic particles like gluons, I’m not trying to make electrons fall into potential wells … instead, I’m doing stuff like clicking the left button of a computer mouse, dragging a file from a folder into the browser window, and so on. This makes it abundantly clear that the level of abstraction having operational significance here is that of computer hardware and computer software.

  

## Okay, But What is an ISA?

Based on what I have described at the start, it is possible to make a computer perform computations, depending on what the CPU is designed to support. However, you would need to know how to ‘activate’ a very specific path in the circuitry of the CPU (overall referred to as its “architecture”) for it to ‘execute’ a certain instruction. And each computation may require more than one instruction, depending on what all you are trying to do, especially if it’s not one but a bunch of computations (to perhaps do a more complicated task).

Indeed, this is a REALLY tedious task, especially since we want this to happen very, very frequently. To solve this annoyance, an Instruction Set Architecture (ISA) is designed: it’s really just a table that maps “the precise ways 0s and 1s can be manipulated in the circuitry” to → “the instructions a human can use to program the computer”.

Let’s say the CPU designer included some storage space (storage units, memory elements). The designer will have reserved some of that real estate for ‘important’ data, like data permanently used for making instructions more efficient. (“Important” is subjective here, it’s ultimately just a design decision that has its own pros and cons.)

I can arbitrarily choose the other, available storage units to keep track of (‘store’) all the money I spent on food this month, and separately all the money I earned. Then, I can even add and subtract those numbers. To do this, I will use instructions of a fake ISA as follows:

| instruction number |  operation  | destination | data source | data source |
| --------------------- | ------------ | ------------ | ------------- | ------------- |
|                1               |     STORE    |     UNIT1     | LOWERBITS |                     |
|                2               |     STORE    |     UNIT2     | LOWERBITS |                     |
|                3               | SUBTRACT  |     UNIT1     |     UNIT1     |      UNIT2     |

Here, I’ve made up a completely fake ISA. In the table, you can see there are two instructions. Both of them do a “STORE” operation, which is rather self-explanatory; to store some data into the space provided by the CPU designer in the CPU, this operation is _required_ as per my fake ISA. The operation is to be accompanied with some destination, and a couple of sources for the data. What numbers am I using? That’s the “data”, and it’s represented in the binary number system (an abstraction), just like the instructions themselves. So, the instruction itself is also a number, meaning we can chop off some of its bits (like chopping off a number’s digits) and still have a number left behind. If I want to put my money spent on biryani in the “UNIT1” storage space, I can encode that amount of money in the instruction itself, and my fake ISA lets me use this encoding to actually store that information in storage!

The second instruction would work similarly, just now storing elsewhere (UNIT2 instead of UNIT1).

  

Let's say that in the first instruction, I stored my money earned in UNIT1, and in the second instruction I stored my money spent on biryani (which is a different amount because the 'lower bits' of this instruction can be different from the previous instruction) in UNIT2. In this context, the third instruction uses the SUBTRACT operation to **_first_** subtract the money spent on biryani from the money earned. The result of this operation will be sent to UNIT1 for storage (because why not? This is design: it makes perfect sense for the result of a mathematical calculation to be immediately sent somewhere for storage purposes). But wait, you might say, doesn't UNIT1 already have something (money earned)? And didn't we mean to use the contents of UNIT1 in this very instruction? Is it valid that we can rewrite UNIT1's contents with the result of this instruction?

Yes! Why use more storage space (e.g., UNIT3)? I expect to earn more money than the cost of biryani, so I expect the result of the instruction containing the SUBTRACT operation to be a large positive number, which represents net profit. So, why not store it the same place where I was storing "money earned" (also a large positive number, hopefully)? This wasn't a design consideration of the CPU or the ISA, it was a design consideration of the **_program_** I wrote using my fake ISA to calculate my net profit — this is what it means to "program a computer"!

  

Let's have a bit of an FAQ to round everything up now that the article is nearing its end:

### What does STORE mean behind the abstraction?

→ although store is an English word in the instruction, it’s defined a bit differently in my fake ISA. It will be a binary number just like everything else is in the CPU’s perspective. This specific number will be used in decision-making when the CPU is trying to execute my instruction: think of it like “if operation in the instruction is STORE then activate the circuitry required to put data into the storage space”. Undoubtedly, there is indeed separate hardware required to implement this decision-making for every instruction, and it’s typically called the decoder (part of the broader control unit). Control is incredibly important for a CPU, but in a general context, it boils down to logic: you have to very carefully, meticulously design the control unit for your hardware. In terms of difficulty, it’s like trying to multiple 74857348.124893 and 238574943.1281923 using only pen and paper (i.e., anybody can do it if they have a lot of time and plan it out really well, but realistically speaking you’d rather literally anything else lol). I would myself prefer a specific context if I want to better understand and appreciate control logic design, like control related considerations in a real processor design, so let’s not awaken the beast in this article!

### What do UNIT1 and UNIT2 actually represent?

→ Storage space in computers need to be interacted with quite extensively, because we want to continuously put stuff in them, take stuff out from them, and use them to move stuff around. As such, each storage unit is given a unique identifier based on its literal physical position. This is called an “address", just like your house address, which is also a unique identifier based on the physical position on your house. Or also like “email address”. Or even like “the President addressed the Senate in his speech”. So, UNIT1 and UNIT2 will also be binary numbers, and there will have to be a carefully designed numbering or ‘addressing’ scheme for all the storage spaces across the CPU, as well as any storage spaces external to the CPU that the CPU can access (like a “USB stick” or “hard drive”).

### How does the computer hardware make sense out of my ‘intention’ of using the instruction’s lower bits as a number (amount of money)?

→ Control unit. That’s it. That’s the tweet.

Oh, and just to be clear: I said the ISA is like a table, and I showed you a table just now, but that table wasn’t actually my fake ISA. That table was just me trying to show you the instruction format of my fake ISA (first row), and how three instructions look like under that format. (As before: that table actually contained a _program_.) The fake ISA as a whole would be a table containing a completely general instruction format, chopping up the total number of bits of the instruction into different chunks, and then assigning meaning to every chunk somehow. It’s all abstract, and all the choices we have while making the ISA are what make it such a powerful tool that requires excellence in the craft to wield it properly.

It’s a little tough trying to ‘get' stuff without concrete examples, so here is a simple and small ISA: [https://user.eng.umd.edu/~blj/RiSC/RiSC-isa.pdf](https://user.eng.umd.edu/~blj/RiSC/RiSC-isa.pdf). Try reading it with the lessons from this article in mind, and you will surely walk away with a respectable understanding!

## What about OS?!

My previous article was a little bit about the OS. Only a little bit. But, at least for now, maybe a balance of software & hardware will be better. If you went through this article, thought about stuff a little bit, and discussed in the comments (perhaps even point out any issues in my explanations), you will actually be in a much more conceptually secure position to better understand OS if and when I eventually dabble in explaining it.

---

P.S. _this writeup was like a compilation of my musings, and is meant for educational purposes only. Errors and omissions are always possible, and I’m open to feedback in comments as well as private messages_.