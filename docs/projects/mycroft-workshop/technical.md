---
layout: default
title: Technical Aspects
parent: Mycroft Workshop
grand_parent: Projects
nav_order: 2
---

# Technical Aspects

If you are comfortable coding in Python, this section is for you. <br>

## Handling intents
Let's say we want to create a method which handles the utterances of a user who want to give up.
In a skill class, the methods for handling intents look like this: <br>
```
    @intent_file_handler("give_up.intent")
    def handle_give_up(self, message):
        animal = self.game_dict["animal"]
        self.speak("Okay, Here is the answer: The animal I was looking for is: {}".format(animal))
```
Let's go through this::
1. The decorator<br>
The decorator `@intent_file_handler` takes in the name of the file which store example utterances for what the user would say. For example, in this case the file would contain utterances like *I want to give up* or *Give me the solution*.

2. The function<br>
The comes the function definition. It takes in the user's utterance (`message`) as an argument. In this case, we don't do anything with it but you could use it to extract more information.
Then we extract some data from a dictionary and finally let Mycroft speak a sentence.

## Making Mycroft speak
There are several different ways for making Mycroft speak, each serving a specific purpose.
- `speak` :<br>
[Documentation](https://mycroft-core.readthedocs.io/en/latest/source/mycroft.html#mycroft.MycroftSkill.speak)<br>
With `self.speak`, Mycroft speaks a given input string.<br>
Example:
```
self.speak("Hello, how are you?")
```
- `speak_dialog`:<br>
[Documentation](https://mycroft-core.readthedocs.io/en/latest/source/mycroft.html#mycroft.MycroftSkill.speak_dialog)<br>
With `self.speak_dialog, Mycroft speaks a random phrase from a dialog file.<br>
Example:
```
self.speak_dialog("hello.dialog")
```
- `get_response`: <br>
[Documentation](https://mycroft-core.readthedocs.io/en/latest/source/mycroft.html#mycroft.MycroftSkill.get_response)<br>
If you expect an immediate reponse from the user, use `self.get_response`. It returns the user's reply to you.<br>
Example:
```
response = self.get_response("What is your favorite animal?")
```
- `ask_yesno`: <br>
[Documentation](https://mycroft-core.readthedocs.io/en/latest/source/mycroft.html#mycroft.MycroftSkill.ask_yesno)<br>
If you want to ask a yes or no question, it is advised to use `self.ask_yesno` as this method also captures reponses like *yeah* or *nah*.<br>
Example:
```
yes_no = self.ask_yesno("Do you like animals?")
```

## Storing persistent data
[Documentation](https://mycroft-ai.gitbook.io/docs/skill-development/skill-structure/filesystem)
As the `__init__.py` file is reloaded each time an utterance is a said by the user, no data that is persistent across utterances can be stored in variables there. Instead, you have to write your data to a file if it changes or is updated across utterances. <br>
It is important that you store data outside the directory your skill is located in if you frequently change it.<br>
Specifically, you need to use the attribute `self.file_system` which returns the path where Mycroft will look for files. For example, if you want to read the file `example.json` that is stored in that directory, you should do something like this:
```
f = f.open(os.path.join(self.file_system, "example.json"))
```
If you are just interested in reading files, you can use the attribute `self.root_dir` to retrieve the path of your skill directory. 
