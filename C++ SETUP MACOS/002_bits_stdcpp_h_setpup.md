## Type 1 :
1. Open Finder :
2. go to Application
3. Xcode : Right click : Show package content
4. cd : content
5. cd : developer
6. cd : Toolchains
7. XcodeDefault.xctoolchain : Right Click : Show Package content
8. cd : usr
9. cd : include

10. Create : `bits` folder and `stdc++.h `
            or
    Copy Paste bits/stdc++.h folder


## Type 2 :
`/usr/local/include/bits/stdc++.h`

1. Open Finder :
2. Show hidden File : (keypress : ) shift + command + . (dot)
3. cd : usr
4. cd : local
5. cd : include

6. Create : `bits` folder and `stdc++.h `
            or
    Copy Paste bits/stdc++.h folder


---

Run :
----
clang++ -I ~/include myfile.cpp -o myfile

Or in Xcode:
	•	Go to Project Settings → Build Settings → Header Search Paths
	•	Add ~/include
