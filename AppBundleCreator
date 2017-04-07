#/bin/bash

# This code written by ILTER ENGIN KIZILGUN
# date Fri Apr 7 04:20:22 +03 2017
# You can modify, change or eat this code
# i hope this piece can help you


function createIconSet() {
	tmp_folder=`mktemp -d`
	tmp_file=`mktemp`.icns
	icon_name=$1

	touch $tmp_file
	mkdir $tmp_folder/icon.iconset
	sips -z 16 16     $icon_name --out $tmp_folder/icon.iconset/icon_16x16.png
	sips -z 32 32     $icon_name --out $tmp_folder/icon.iconset/icon_16x16@2x.png
	sips -z 32 32     $icon_name --out $tmp_folder/icon.iconset/icon_32x32.png
	sips -z 64 64     $icon_name --out $tmp_folder/icon.iconset/icon_32x32@2x.png
	sips -z 128 128   $icon_name --out $tmp_folder/icon.iconset/icon_128x128.png
	sips -z 256 256   $icon_name --out $tmp_folder/icon.iconset/icon_128x128@2x.png
	sips -z 256 256   $icon_name --out $tmp_folder/icon.iconset/icon_256x256.png
	sips -z 512 512   $icon_name --out $tmp_folder/icon.iconset/icon_256x256@2x.png
	sips -z 512 512   $icon_name --out $tmp_folder/icon.iconset/icon_512x512.png
	cp $icon_name $tmp_folder/icon.iconset/icon_512x512@2x.png
	iconutil -c icns $tmp_folder/icon.iconset
	mv $tmp_folder/icon.icns $tmp_file
	rm -R $tmp_folder/icon.iconset
	echo $tmp_file 
}




tmp_folder=`mktemp -d`

# create project template
mkdir $tmp_folder/Contents
mkdir $tmp_folder/Contents/MacOS
mkdir $tmp_folder/Contents/Resources
touch $tmp_folder/Contents/Info.plist
touch $tmp_folder/Contents/PkgInfo
touch $tmp_folder/Contents/MacOS/RunIT

PLIST_FILE=$tmp_folder/Contents/Info.plist


echo -n "Application Name: "
read APP_NAME
echo -n "Application Identifier: "
read APP_IDENT
echo -n "CopyRight/Left: "
read APP_CR
echo -n "Icon File Path: "
read APP_ICON
echo -n "Project Root Folder: "
read APP_PROJ_ROOT
echo -n "Executable Name: "
read APP_EXEC_NAME


if [ -f "$APP_ICON" ]
	then
	ICNS_NAME=$(createIconSet "$APP_ICON" | tail -n 1)
	mv $ICNS_NAME $tmp_folder/Contents/Resources/icon.icns
fi	


if [[ -d "$APP_PROJ_ROOT" && -f "$APP_PROJ_ROOT/$APP_EXEC_NAME" ]]
	then
	cp -a "$APP_PROJ_ROOT" $tmp_folder/Contents/MacOS/
	echo "#!/bin/sh" >> $tmp_folder/Contents/MacOS/RunIT
	echo "/usr/bin/open $APP_EXEC_NAME \"\$@\"" >> $tmp_folder/Contents/MacOS/RunIT
	echo "exit 0" >> $tmp_folder/Contents/MacOS/RunIT
	chmod +x $tmp_folder/Contents/MacOS/RunIT
	
else 
	echo "Project Folder: $APP_PROJ_ROOT"
	echo "Executable Folder: $APP_PROJ_ROOT/$APP_EXEC_NAME"
	echo "Project folder or executable broken"
	exit 1
fi	




		
# create plist file
echo '<?xml version="1.0" encoding="UTF-8"?>' >> $PLIST_FILE
echo '<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">' >> $PLIST_FILE
echo '<plist version="1.0">' >> $PLIST_FILE
echo '	<dict>' >> $PLIST_FILE
echo '		<key>CFBundleDisplayName</key>' >> $PLIST_FILE
echo "		<string>$APP_NAME</string>" >> $PLIST_FILE
echo '		<key>NSHumanReadableCopyright</key>' >> $PLIST_FILE
echo "		<string>$APP_CR</string>" >> $PLIST_FILE
echo '		<key>CFBundleExecutable</key>' >> $PLIST_FILE
echo "		<string>RunIT</string>" >> $PLIST_FILE
echo "		<key>CFBundleIconFile</key>" >> $PLIST_FILE
echo '		<string>icon.icns</string>' >> $PLIST_FILE
echo "		<key>CFBundleIdentifier</key>" >> $PLIST_FILE
echo "		<string>$APP_IDENT</string>" >> $PLIST_FILE
echo '		<key>CFBundleDocumentTypes</key>' >> $PLIST_FILE
echo "		<array></array>" >> $PLIST_FILE
echo '		<key>CFBundlePackageType</key>' >> $PLIST_FILE
echo "		<string>APPL</string>" >> $PLIST_FILE
echo '		<key>IFMajorVersion</key>' >> $PLIST_FILE
echo "		<integer>0</integer>" >> $PLIST_FILE
echo '		<key>IFMinorVersion</key>' >> $PLIST_FILE
echo "		<integer>1</integer>" >> $PLIST_FILE
echo '	</dict>' >> $PLIST_FILE
echo '</plist>' >> $PLIST_FILE

# create PkgInfo
echo "APPL$APP_NAME" >> $tmp_folder/Contents/PkgInfo


mv $tmp_folder "$APP_NAME.app"

