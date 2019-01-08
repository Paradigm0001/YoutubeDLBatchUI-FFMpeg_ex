# **YoutubeDLBatchGUI**
## Credit: https://youtube-dl.org/
#### A simple batch GUI for the very popular "Youtube-DL.exe" application
#### FFMpeg included version: https://github.com/Paradigm0001/YoutubeDLBatchUI
#### This is the FFMpeg excluded version, This means:
- This version does not support audio downloading out of the box
- It is smaller than the FFMpeg included version
- Has options to connect to an already installed FFMpeg (Saving you space incase its already installed)

##### Below will be ```source code``` of the YOUTUBE DOWNLOADER.bat ~~for the paranoid~~:
```bat
@echo off
youtube-dl.exe -U >nul 2>&1
setlocal enabledelayedexpansion
set VERBOSE_STATE=Disabled
title Youtube Video Downloader GUI
if not exist "%CD%\credit" (
	echo This application was not created by me
	echo:
	echo This is just a simple GUI for it, you can find the original at: https://youtube-dl.org/
	pause
	echo Paradigm#0001 on discord>credit
	cls
)
if exist "%CD%\FFMPEG_DIR" (
	set /p FFmpegDIR=<"%cd%\FFMPEG_DIR"
) else goto FFmpegDIRSETUP
:menu
mode 75, 10
echo 1. Download Youtube video
echo 2. Download Youtube audio
echo 3. Toggle Debug mode [Currently !VERBOSE_STATE!]
echo 4. Set FFmpeg directory
echo:
echo:
echo:
echo 8. Open Output folder
echo 9. Display help [Opens external text editor]
choice /c 123456789 /n /m "Select an option: "
cls
if %ERRORLEVEL%==1 (
	set /p VIDEO_URL="Please insert video URL: "
	youtube-dl.exe -i %VERBOSE% --geo-bypass --yes-playlist --age-limit 25 --console-title !VIDEO_URL!
	title Youtube Video Downloader GUI
	goto menu
) else if %ERRORLEVEL%==2 (
	:audio_download
	echo [Type "list" for a list of accepted formats]
	echo [This option requires FFmpeg to function, Type "skip" if you lack ffmpeg]
	set /p FORMAT="Enter the desired format: "
	if /i !FORMAT!==list (
		cls
		echo Accepted formats [Highest quallity to lowest]:
		echo aac
		echo flac
		echo wav
		echo m4a
		echo opus
		echo vorbis^[OGG^]
		echo mp3
		pause
		cls
		set FORMAT=
		goto audio_download
	)
	if /i !FORMAT!==aac (
		set AUDIO_pass=1
	) else if /i !FORMAT!==flac (
		set AUDIO_pass=1
	) else if /i !FORMAT!==wav (
		set AUDIO_pass=1
	) else if /i !FORMAT!==m4a (
		set AUDIO_pass=1
	) else if /i !FORMAT!==opus (
		set AUDIO_pass=1
	) else if /i !FORMAT!==vorbis (
		set AUDIO_pass=1
	) else if /i !FORMAT!==mp3 (
		set AUDIO_pass=1
	) else if /i !FORMAT!==skip (
		set FORMAT= & set AUDIO_pass=1
	) 
	if !AUDIO_pass!==1 (
		goto :AUDIO_pass
	)
	cls & echo Invalid format selected & pause & cls & goto audio_download
	:AUDIO_pass
    set "FORMAT=--audio-format !FORMAT!"
	cls
	set /p Audio_Quallity="Enter audio quallity [0 to 9; 0 best, 9 worse]: "
	cls
    echo [To save to default dir; input nothing][Default save dir below:]
    echo "%CD%\output"
    echo:
    if not defined SAVE_DIR set SAVE_DIR=./output
    set /p SAVE_DIR="Please enter a save directory: "    
    cls
	set /p VIDEO_URL="Please insert video URL: "
    mode 75, 50
	youtube-dl.exe -x -i %VERBOSE% --ffmpeg-location "!FFmpegDIR!" !FORMAT! --audio-quality !Audio_Quallity! -o "!SAVE_DIR!/%%(title)s-%%(id)s.%%(ext)s" --ignore-config --geo-bypass --yes-playlist --age-limit 25 --console-title !VIDEO_URL!
	mode 75, 10
    title Youtube Video Downloader GUI
	goto menu
) else if %ERRORLEVEL%==3 (
	if %VERBOSE_STATE%==Disabled (
		set "VERBOSE=-v" & set "VERBOSE_STATE=Enabled"
	) else set "VERBOSE=" & set "VERBOSE_STATE=Disabled"
	goto menu
) else if %ERRORLEVEL%==4 (
	:FFmpegDIRSETUP
	cls
	echo [Example: C:\Program Files\FFmpeg\bin\ffmpeg.exe]
    echo [MUST point to FFMpeg\bin or FFmpeg.exe]
	echo [Type "skip" to skip setting up FFmpeg]
    echo:
	set /p FFmpegDIRtemp="Enter the directory pointing to the FFmpeg binaries: "
	if /i !FFmpegDIRtemp!==skip goto menu
	if not exist !FFmpegDIRtemp! echo: & echo Invalid Directory; Directory does not exist & pause & goto FFmpegDIRSETUP
	set FFmpegDIR=!FFmpegDIRtemp!
	echo !FFmpegDIR!>FFMPEG_DIR
	cls
	goto menu
) else if %ERRORLEVEL%==8 (
    if not defined SAVE_DIR (start "" "%cd%\output") else start !SAVE_DIR!
    goto menu
) else if %ERRORLEVEL%==9 (
	youtube-dl.exe -h>>temp.txt
	start temp.txt
	ping 127.0.0.1 -n 2 >nul
	del temp.txt
	goto menu
) else goto menu
```
