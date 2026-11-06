# pbskill

`pbskill.jar` checks skills with the same rules the app uses, so a pull request is held to exactly what Peanut Butter will accept. It contains the checker and the page generator only.

```sh
java -jar tools/pbskill.jar check skills/*.pbskill
java -jar tools/pbskill.jar index skills site https://skills.penotbota.com
```

It is updated here whenever the skill format changes. Requires Java 21.

It is not covered by this repository's MIT License. You may run it to check and publish skills; see [LICENSE](LICENSE).
