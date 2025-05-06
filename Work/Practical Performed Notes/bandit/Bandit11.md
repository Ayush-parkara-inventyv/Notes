ssh bandit11
**ssh bandit11@bandit.labs.overthewire.org -p 2220**
![[Pasted image 20241218160746.png]]
first i checked the hint that the characters are shifted 13 pos and then checked the material
it gave me complete understanding as well as the concept that we just need the text from data.txt and then feed it to the **tr** and then give the values for each alphabet
so as it was said a-z and A-Z are used its 13th pos will be 
**N-Z** for **A-M** and **n-z** for **a-m**
so did the same as said above in this note
**cat data.txt | tr "A-Za-z" "N-ZA-Mn-za-m"**
first i used cat to display the text and did pipeline to convert it into normal from ROT13
password:**7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4**
![[Pasted image 20241218161322.png]]