### Building your own dolphin.exe
 Github will build a new dolphin.exe for every change you make in the Dolphin folder as long as you have actions enabled on your repository. 

 If you want to customize the dolphin icon put a custom .ico file in the Resources folder then head to Dolphin/Sys/Resources and customize Dolphin.png, dolphin_logo.png, and dolphin_logo@2x.png.

 After uploading your changes to github, actions will start building your dolphin.exe. Go to your build's github page and click on the actions tab. You should see it start building. After it's done it'll post a release you can download.
 
 Download the zip and put the sd.raw you built with the CreateSD.bat script in the User/Wii folder.
 
## How to make a custom dolphin updater
 By default github will include an Updater.exe with your dolphin download. If you want to customize where the updater looks for updates follow these steps.
 
 First you need to make the update files. Run the "Delete files (Update).bat" included in the Dolphin folder to remove all files not needed for the update. Then put your sd.raw in the User/Wii folder and zip the files up.
 
 Open ".github/workflows/pr-build.yml" in a text editor and between where it says '- name: "Clone Dolphin"' and '- name: "Build Dolphin"' put this:
 
       - name: "Replace IDs"
        working-directory: ${{ github.workspace }}
        shell: powershell
        run: |
          (Get-Content ${{ github.workspace }}\Ishiiruka\Source\Core\DolphinWX\Main.cpp) -replace 'https://projectplusgame.com/update.json', 'https://raw.githubusercontent.com/jlambert360/PPlus-Build-Template/main/Resources/update.json' | Out-File -encoding ASCII ${{ github.workspace }}\Ishiiruka\Source\Core\DolphinWX\Main.cpp

 Replace 'https://raw.githubusercontent.com/jlambert360/PPlus-Build-Template/main/Resources/update.json' with a link to the json file in your repository Resources folder. To get the raw link just navigate to it on your github page and hit the button that says raw, or just replace my username and repository name with yours.
 
 To get the hash go here: https://github.com/jlambert360/Ishiiruka/commits/master and hit the 2 squares on the right of the screen on the latest commit to copy the latest hash. You can compare this in dolphin in help->about under Revision.
 
 Version is whatever is displayed after Ishiiruka-Dolphin in the top bar of dolphin.
 
 Changelog is whatever changes you made to your build. You can write whatever you want here.
 
 Download-page-windows/mac is whatever the link to your update files are. I reccommend hosting them on github releases to get a direct link if your build is under 2GB.