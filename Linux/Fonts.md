# Fonts

Premise:
```shell
distro: Kubuntu 22.04.1 LTS x86_64    
kernel: 5.15.0-58-generic    
shell: bash 5.1.16    
de: Plasma 5.24.7    
wm: KWin
```  

You can install OTF (OpenType Font) or TTF (TrueType Font) files - preferred OTF.

Variables for easier copy and paste of code snippets. Don't use spaces in variables.

```shell
FONT_NAME=
```  

## Paths / Locations

```shell
/usr/share/fonts
/usr/local/share/fonts
~/.fonts
~/.local/share/fonts
```

## User specific installation (no root)

1. Download the \*.otf or \*.ttf font files.
2. Create fonts folder:  
```shell
mkdir -p ~/.local/share/fonts/$FONT_NAME
```  
3. Move all font files to the folder:  
```shell
mv *.otf ~/.local/share/fonts/$FONT_NAME
or
mv *.ttf ~/.local/share/fonts/$FONT_NAME
```  
4. Optional - rebuild the font cache:  
```shell
fc-cache -f -v
```  
5. Optional - check if font is installed:  
```shell
fc-list | grep "font name"
```  

## System wide installation (root)

1. Download the \*.otf or \*.ttf font files.
2. Create fonts folder:  
```shell
sudo mkdir -p /usr/local/share/fonts/$FONT_NAME
```  
3. Move all font files to the folder:  
```shell
sudo mv *.otf /usr/local/share/fonts/$FONT_NAME
or
sudo mv *.ttf /usr/local/share/fonts/$FONT_NAME
```  
4. Reboot or rebuild the font cache manually:  
```shell
sudo fc-cache -f -v
```  
5. Optional - check if font is installed:  
```shell
fc-list | grep "font name"
```  

## Font: Source Code Pro 

Source Code Pro is an open-source font family from Adobe. It's very common for coding and the terminal.

[Source Code Pro on GitHub](https://github.com/adobe-fonts/source-code-pro) 
[Specimen](https://adobe-fonts.github.io/source-code-pro/)

**User specific installation:**  
```shell
mkdir -p ~/.local/share/fonts/source_code_pro && \
wget -P ~/.local/share/fonts/source_code_pro \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Black.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-BlackIt.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Bold.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Bold.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-ExtraLight.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-ExtraLightIt.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-It.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Light.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-LightIt.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Medium.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-MediumIt.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Regular.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Semibold.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-SemiboldIt.otf
```  

**System wide installation:**  
```shell
sudo mkdir -p /usr/local/share/fonts/source_code_pro && \
sudo wget -P /usr/local/share/fonts/source_code_pro \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Black.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-BlackIt.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Bold.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Bold.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-ExtraLight.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-ExtraLightIt.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-It.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Light.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-LightIt.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Medium.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-MediumIt.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Regular.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-Semibold.otf \
https://github.com/adobe-fonts/source-code-pro/raw/release/OTF/SourceCodePro-SemiboldIt.otf && \
sudo fc-cache -f -v
```  

Links:

- [Nerd Fonts](https://www.nerdfonts.com)
- [Source Code Pro](https://github.com/adobe-fonts/source-code-pro) 
