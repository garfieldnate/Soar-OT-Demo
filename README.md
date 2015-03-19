Soar Optimality Theory Demo
===========================

This is a small demonstration of optimality theory implemented in Soar. I did it as a syntax class assignment as an undergraduate, and presented it at the [Soar workshop](http://ai.eecs.umich.edu/soar/sitemaker/workshop/32/). It's not quite polished, but I hope to make it more useful.

What is Soar?
-------------
Soar is a general cognitive architecture used for creating and studying agents that exhibit intelligent behavior. It works by proposing, selecting, and executing operators, and working in sub-spaces when current knowledge is not enough to continue with this cycle.

What is Optimality Theory?
--------------------------

[Optimality theory](http://en.wikipedia.org/wiki/Optimality_theory) (or OT) is a simple theory of linguistic phenomena which works by using ranked, violable constraints. The relative rank of a constraint represents the markedness of some feature, whether it is phonological, syntactic or other. Predictions or explanations of behavior are then determined by enumerating all of the choices presented to a speaker and choosing the one which violates the least marked constraints. This is normally represented in the OT literature with a "tableau" (a table with finger pointing to the predicted behavior). 

OT provides an elegant mechanism for modeling  cross-lingusitic or cross-dialectic similarity and variation. While the constraints themselves are held to be present in a greater population, the relative markedness of each is held to vary between communities of speakers, and this variation leads to variation in behavior.

So What's this Project?
---------------------
This is a small demo of OT implemented in Soar. OT turns out to be easily implemented in Soar using operator preferences. 

The demonstrated domain is that of "quirky case" in Japanese.

Japanese contains several case particles: は(topical), が (nominative), に (dative) and を (accusative) are the relevant examples for this application. There is, however, a problem with giving each case marker a label that implies a specific position in syntactic structure. Japanese contains verbs with inherent and/or lexical case (referred to as "quirky case"). These verbs license different case marking, leading to dative subjects and nominative objects. A comprehensive treatment of these verbs requires a large knowledge source detailing the selectional requirements of every verb in Japanese, so in this demo I treat a small number of lexemes by coding information about them directly into productions. I also make use of the morpheme 'e' ('can'), which attaches to verbs and always licenses quirky case. Woolford showed that transitivity has an effect on the licensing of quirky case. The following is from Woolford (2006) via Shibatani (1973):

A. 太郎に英語が話せる (Taro-dat English-nom speak-can; Taro can speak English)
B.* 赤ちゃんにもう歩ける (baby-dat already walk-can; the baby can already walk)
C. 赤ちゃんがもう歩ける (baby-nom already walk-can; the baby can already walk)

Even though the first sentence has the 'can' morpheme, which licenses quirky case, the verb it attaches to is intransitive, which disallows quirky case. Woolford (2001) gives an OT account for this data. She sets forth violable constraints that account for the correct distribution of irregular case in Japanese and many other languages. In Japanese they are thus:

    Faith-lextrans >> *dat >> Faith-lex >> *acc >> *nom

*dat, *acc and *nom encode relative markedness of the three cases (dative, nominative and accusative), and they are proposed to be universal across languages. Each of the Faith-lex constraints check the irregular case specified by the verb; faith-lextrans applies only for transitive verbs, so that only in transitive verbs is faithfulness to the irregular case respected, while the markedness of dative case is a greater constraint than faithfulness in intransitive constructions. The following are two Japanese examples showing how this works:

分かる (transitive, quirky case) 

| predicted behavior|  Faith-lex<sub>trans</sub> | *dat | Faith-lex | *acc | *nom |
|-------------------|----------|------|-----------|------|---|
|☞ subj-dat obj-nom |          |   *  |           |      | * |
|subj-dat obj-acc    |          |   *  |           |  *!  |   |   
|subj-nom obj-acc    |    *!    |      |     *     |   *  |   |   

思える (intransitive, quirky case) 

|predicted behavior| Faith-lex <sub>trans</sub> | *dat  |  Faith-lex |   *acc  |  *nom |
|------------------|---|----|---|---|---|
|subj-dat obj-nom  |   | *! |   |   | * |
|subj-dat obj-acc  |   | *! |   | * |   |
|☞subj-nom obj-acc|   |    | * | * |   |

Following Woolford’s account of Japanese case assignment, I implemented a simple demo for my OT application in Soar. Recognition of violations of *dat, *acc and *nom use only information about the case assignment that an operator is proposing. Both of the Faith-lex violation recognition productions use this information, and also information encoded on an input word, the valency and whether or not quirky case is licensed. 

Word  | Valency      | Quriky Case?
------|--------------|-------------
wakar | Transitive   | Yes
aruk  | Intransitive | No
koros | Transitive   | No

The following are step-by-step instructions on how to run the demo.

1. Open the Soar Debugger and [SoarTextIO](http://soar.eecs.umich.edu/articles/downloads/domains/175-soartext-io). The Soar Debugger is located in the root of the Soar distribution, but unfortunately SoarTextIO no longer comes with Soar, so you will have to obtain it elsewhere or build it from [source](https://github.com/SoarGroup/Domains-SoarTextIO).
2. Using the Soar debugger, source the file called “CaseOT.soar” from the project’s root directory.
3. In the SoarTextIO window, try typing one of the vocabulary from the table above, and then clicking “Run” in the Soar debugger. After the agent halts, you should see a message stating what the decision on case assignment was.
4. An unfortunate side effect of this limited application is that each time you wish to run the application with a different lexeme, you must close SoarTextIO and restart it. Reopen SoarTextIO and try entering one of the lexemes, followed by a space and the letter “e” (the “can” morpheme in Japanese). Because this morpheme licenses quirky case on the verb it is attached to, it may change the case assignment. The table below lists all of the possible combinations of input and the assigned case. Note that "aruk" is considered intransitive in this system, so object case assignment is meaningless. Also, "wakar e" is not a natural Japanese phrase; ignoring it has no effect on the validity of the system.

Input   | Subject case  | Object case
--------|---------------|------------
wakar   |      dat      |     nom
*wakar e|      dat      |     nom
aruk    |      nom      |     *acc
aruk e  |      nom      |     *acc
koros   |      nom      |     acc
koros e |      dat      |     nom

## References
* Shibatani, M. (1973). Semantics of Japanese Causitivization. Foundations of Language, 9(3), 327-373. Retrieved from http://www.jstor.org/stable/25000675
* Woolford, E. (2006). Lexical Case, Inherent Case, and Argument Structure. Linguistic Inquiry, 37(1), 111--130.
