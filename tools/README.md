# pbskill

`pbskill.jar` is Peanut Butter's own skill validator, packaged so this repository can check skills with exactly the code the app runs.

```sh
java -jar tools/pbskill.jar check skills/*.pbskill
java -jar tools/pbskill.jar index skills site https://skills.penotbota.com
```

It is built from the Peanut Butter engine with `./gradlew :engine:pbskillJar` and copied here whenever the skill format changes. Requires Java 21.
