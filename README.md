# Koine Verb Viz
This is a simple visualization tool to show how toggling different elements of a Koine Greek verb (tense, voice, and mood) affect its form and meaning.

You can limit which moods are displayed by adding a comma-separated `moods` value to the query string. For example:
`https://localhost:8080?moods=Indicative,Subjunctive`
will display only the indicative and subjunctive moods.

To get going, it's just:
`npm install`
and then:
`npm run serve`
