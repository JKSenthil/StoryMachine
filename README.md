# StoryMachine

# Project Title and purpose

One Paragraph of project description goes here

### Difficulties or opportunities you encountered along the way.

The toughest part was...

### Most interesting piece of your code and explanation for what it does.

```Java
import java.util.*;
import java.util.regex.*;

Map<String, String> map;
String regex = "(\\w+)";
String story;
void setup() {
  map = new HashMap<String, String>();
  init();

  String[] s_array = loadStrings("data/TandH.txt");
  story = "";
  for(String s : s_array){
    story += s + "\n";
  }
  
  Pattern p = Pattern.compile(regex);
  Matcher m = p.matcher(story);
  String[] sss = {"syn", "nry", "hom"};
  while (m.find()) {
    String s = m.group();
    String s1 = "";
    if (map.containsKey(s)) {
      s1 = map.get(s);
    } else {
      int i = (int) random(0,3);
      JSONArray jsonArray = loadJSONArray("https://api.datamuse.com/words?rel_" + sss[i] + "="+s);
      int size = jsonArray.size();
      int num = (int) (Math.random() * size);
      if (num == 0) {
        s1 = s;
      } else { 
        JSONObject jsonObject = jsonArray.getJSONObject(num);
        s1 = jsonObject.getString("word");
      }
      map.put(s, s1);
    }
    story = story.replaceFirst(s, s1);
  }
  capitalizeStory();
  println(story);
}

void init() {
  String[] w = loadStrings("data/whitelist.txt")[0].split(" ");
  for (String s : w) {
    map.put(s, s);
  }
}

void capitalizeStory(){
  story = story.substring(0,1).toUpperCase() + story.substring(1);
  for(int i = 0; i < story.length(); i++){
    Character character = story.charAt(i);
    if(character.equals('.')){
      if(i + 2 < story.length()){
        i+=2;
        story = story.substring(0,i) + story.substring(i,i+1).toUpperCase() + story.substring(i+1);
      }
    }
  }
}
```
This is the code that moves down the tree as decisions are made.  It gets each value from both left and right and also casts the value to a String.  If the progressions arrives at the leaf nodes, those values are printed.
## Built With

* [Processing](https://processing.org/) - The IDE used

## Authors

* **Billie Thompson** 


## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc
