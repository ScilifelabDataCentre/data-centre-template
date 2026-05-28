What to do if Cspell flags a word that you know is correct:
1. Check if Cspell has a dictionary to import
npx cspell trace --config .config/cspell.json [word]
2. If there's an asterisk (*) next to a dictionary, add the 