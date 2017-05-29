# Electron-tutorial

[**원본강의**](http://cionman.tistory.com/7)에서 소개하는 Electorn 을 간단히 사용해 보자


## Electron은 무엇인가?
[**Electron**](https://electron.atom.io/)은 **HTML, css, javascript**을 사용하여 **크로스 플랫폼**을 지원하는 **데스크탑 어플리케이션**을 제작할 수 있는 프레임워크이며 [Chromium](https://www.chromium.org/)과 [Node.js](https://nodejs.org/)를 하나의 런타임으로 결합하여 실행되며, Mac, Windows 및 Linux 용으로 패키지화 할 수 있습니다.
<br/>

## Electron 설치
---
1. Node 설치
2. Electron 설치
```
npm install electron --save
```
---

## Electron 실행 구조

**Electron**은 Main Process와 Renderer Process라고 불리는 2개의 실행 구조를 가집니다.
1. Main Process : 이 프로세스는 **Electron**의 주 프로세스이며, [BrowserWindow](https://electron.atom.io/docs/api/browser-window/) api 인스턴스를 통해 창을 생성하고 웹페이지를 읽어옵니다. Browser process 라고도 불립니다.<br/><br/>
 대표 API 소개
    - [app](https://electron.atom.io/docs/api/app/) : 어플리케이션의 생명주기를 다루는 모듈
    - [BrowserWindow](https://electron.atom.io/docs/api/browser-window/) : 창을 생성하고 컨트롤하는 모듈
    - [Menu](https://electron.atom.io/docs/api/menu/) : 어플리케이션의 네이티브 메뉴를 생성하는 모듈
    - [dialog](https://electron.atom.io/docs/api/dialog/) : 네이티브 시스템 다이얼로그 창을 생성하는 모듈(file, alert 등)
    - [ipcMain](https://electron.atom.io/docs/api/ipc-main/) : Main Process에서 Renderer Process로 비동기적으로 통신하는 모듈
    - [Tray](https://electron.atom.io/docs/api/tray/) : 
    시스템의 알림영역에 아이콘 및 컨텍스트 메뉴를 추가하는 모듈
2. Renderer Process : Main Process의 BrowserWindow가 표시하는 웹페이지 내의 프로세스 입니다. 각각의 웹페이지는 개별적인 Renderer Process를 가집니다.<br/></br>
일반 브라우저는 시스템의 고유 리소스 접근을 허용하지 않지만, **Electron**은 [Node.js](https://nodejs.org) API를 통해 OS와 상호작용을 가능하게 합니다.<br/><br/>
대표 API 소개
    - [webFrame](https://electron.atom.io/docs/api/web-frame/) : 현재 웹페이지의 렌더링을 커스터마이징하는 모듈
    - [remote](https://electron.atom.io/docs/api/remote/) : Renderer Process에서 Main Process의 모듈을 사용이 가능하게 해주는 모듈
    - [ipcRenderer](https://electron.atom.io/docs/api/ipc-renderer/) : Renderer process에서 Main Process로 비동기적으로 통신하는 모듈

# 프로젝트 만들기
## Project폴더 생성 후 package.json 작성하기
앞서 Node.js를 설치 하였기 때문에 **npm init** 명령어가 가능합니다.
커맨드 창에서 프로젝트 경로로 진입 한 후 **npm init -y** 명령어로 기본 package.json 파일을 생성합니다. -y 옵션은 기본값으로 package.json을 생성하는 옵션입니다.
<br />
**package.json**
```
     {
        "name": "Electron-HelloWorld",
        "version": "1.0.0",
        "description": "",
        "main": "index.js", //엔트리 포인트
        "scripts": {
            "start": "electron ."  // 실행 스크립트
        },
        "author": "tintoll <belselios@gmail.com>", //리눅스 빌드시 필요함
        "license": "ISC"
    }
```
## 실행
```
     npm start
```

# Electron 실행 파일 만들기
## 1. electron-builder 설치

installer파일을 만들기 위해서 npm으로 electron-builder을 설치해야 합니다.
커맨드 창에서 아래의 명령어를 실행합니다.

**electron-builder설치**
```
     npm install --save-dev electron-builder
```
## 2. npm script 작성

package.json scripts의 하위 항목에 아래 내용을 추가합니다.

아래의 내용은 커맨드 창에서 입력할수 있는 명령어를 scripts 하위항목에 추가하는 내용입니다. 옵션에대한 좀더 자세한 내용은 아래 링크를 참조 바랍니다.

[CLI 상세 옵션](https://github.com/electron-userland/electron-builder#cli-usage)

**package.json**
```
    "build:osx": "build --mac",
    "build:linux": "npm run build:linux32 && npm run build:linux64",
    "build:linux32": "build --linux --ia32",
    "build:linux64": "build --linux --x64",
    "build:win": "npm run build:win32 && npm run build:win64", 
    "build:win32": "build --win --ia32",
    "build:win64": "build --win --x64"
```

## 3. build option 작성

package.json의 최상위 항목에 아래 내용을 추가합니다.

[build options 상세내용](https://github.com/electron-userland/electron-builder/wiki/Options)

**package.json**
```
    ...
    "build": {
    "productName": "HelloElectron",
    "appId": "com.electron.hello",
    "asar": true, //소스코드를 asar 포맷으로 압축 패키징 옵션
    "protocols" : {
        "name" : "helloElectron",
        "schemes" : ["helloelectron"]
    },
    "mac": { //mac용 옵션
      "target": [
        "default"
      ],
      "icon": "./resources/installer/Icon.icns"
    },
    "dmg": { //mac 인스톨 옵션
      "title": "HelloElectron",
      "icon": "./resources/installer/Icon.icns"
    },
    "win": {  // windows 옵션
      "target": [  //
        "zip",  // zip
        "nsis"  // 인스톨러 실행파일
      ],
      "icon": "./resources/installer/Icon.ico"
    },
    "linux": { //리눅스 옵션
      "target": [
        "AppImage", 
        "deb",
        "rpm",
        "zip",
        "tar.gz"
      ],
      "icon": "./resources/linuxicon"
    },
    "nsis":{
      "oneClick" : false, //nsis 기본 옵션은 원클릭 true
      "allowToChangeInstallationDirectory" :true // 디렉토리 변경 옵션
    },
    "directories": {
      "buildResources": "resources/installer/",
      "output": "dist/", // 빌드 후 결과물 저장 경로
      "app": "."
    }
  },
  ...
```
## 4. build os별 빌드 실행

**Mac 터미널**
```
npm run build:osx
```
**Windows 명령 프롬프트(CMD)**
```
npm run build:win
```
**Linux 터미널**
```
npm run build:linux
```
## 5. Multi Platform Build 설정방법
기본적으로 빌드를 실행하는 운영체제의 결과물만 정상적으로 빌드가 됩니다. 하지만 하나의 운영체제에서 다른 OS플랫폼의 빌드를 실행하려면 아래와 같은 준비가 필요합니다.

**Mac**
1. HomeBrew를 설치 합니다. [HomeBrew설치법 Blog](http://humble.tistory.com/26)
2. Mac에서 Windows를 build하기 위하여 brew를 통해 아래의 패키지를 설치합니다.
```
brew install wine --without-x11
brew install mono
```
3. Mac에서 Linux를 build하기 위하여 brew를 통해 아래의 패키지를 설치합니다.
```
brew install gnu-tar graphicsmagick xz
brew install rpm
```
**Linux**
1. Linux에서 Windows를 build하기 위하여 아래의 패키지를 설치합니다.

```
sudo add-apt-repository ppa:ubuntu-wine/ppa -y
sudo apt-get update
sudo apt-get install --no-install-recommends -y wine1.8
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
echo "deb http://download.mono-project.com/repo/debian wheezy main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list
sudo apt-get update
sudo apt-get install --no-install-recommends -y mono-devel ca-certificates-mono
```

**Windows**
Docker를 사용합니다.

[Multi Platform Build에 관련 내용](https://github.com/electron-userland/electron-builder/wiki/Multi-Platform-Build)

## 6. Mutli Pltform Build 실행

package.json 파일의 scripts 부분에 아래의 명령어를 추가합니다.

**package.json**
```
    "build": "npm run build:linux && npm run build:osx && npm run build:win"
```
커맨드 창에서 아래와 같이 실행합니다.
```
npm run build
```


## 주요 API 
Electron API 를 사용해보는 간단한 예제 입니다.

## Menu(Main), shell(Both), dialog(Main) 활용 예제

 ### 메뉴를 생성하고 해당 메뉴에서 몇가지 기능을 실행해 봅니다.

[Menu](https://electron.atom.io/docs/api/menu/) - 어플리케이션의 메뉴를 생성하는 API

[shell](https://electron.atom.io/docs/api/shell/) - file과 url을 사용자의 기본 어플리케이션으로 실행 및 관리해주는 API

[dialog](https://electron.atom.io/docs/api/dialog/) - alert 및 file dialog창 생성 


 1. 아래와 같이 menu.js파일을 생성합니다.
변수 arrMenu에 해당하는 하나의 객체는 MenuItem 객체입니다.

[MenuItem 객체 상세옵션](https://electron.atom.io/docs/api/menu-item/)

**menu.js**

```
const {app, shell, dialog, Menu, BrowserWindow} = require('electron');
const appName = app.getName();

let arrMenu = [
	{
		label: '편집',
		submenu: [
			{
				label: '실행취소',
				role: 'undo'
			},
			{
				label:'다시실행',
				role: 'redo'
			},
			{
				type: 'separator'
			},
			{
				label:'잘라내기',
				role: 'cut'
			},
			{
				label:'복사',
				role: 'copy'
			},
			{
				label:'붙여넣기',
				role: 'paste'
			},
			{
				label:'모두선택',
				role: 'selectall'
			},
			{
				label:'삭제',
				role: 'delete'
			}
		]
	},
	{
		label: '창',
		role: 'window',
		submenu: [
			{
				label: '최소화',
				accelerator: 'CmdOrCtrl+M',
				role: 'minimize'
			},
			{
				label: '&닫기',
				accelerator: 'CmdOrCtrl+W',
				role: 'close'
			},
			{
				type: 'separator'
			},
			{
				role: 'togglefullscreen'
			}
		]
	},

	{
		label: '사이트',
		role: 'help'
	},
	{
		label:'포탈',
		submenu:[
			{
                label: '&네이버'
                ,click() {
                shell.openExternal('http://naver.com');
            	}
			}
		]
	},
    {
        label:'클릭',
        submenu:[
            {
                label: '다이얼로그창을 보여주세요'
                ,click() {
                dialog.showMessageBox({ message: "Electron의 dialog.showMessageBox 창!!",

                    buttons: ["확인"]
                });

                }
            },
            {
                label: '제목에 붉은배경색 칠하기'
                ,click() {
                BrowserWindow.getFocusedWindow().webContents.executeJavaScript('changeColor()');

            }
            }
        ]
    }

];
//Node.js 전역객체 인 process객체를 통해 OS를 구분하여 메뉴를 추가
if (process.platform === 'darwin') {
    arrMenu.unshift({
		label: appName,
		submenu: [
			{
        label:'프로그램 종료',
				role: 'quit'
			}
		]
	});
} else {
    arrMenu.unshift({
		label: '파일',
		submenu: [
			{
				label:'프로그램 종료',
				role: 'quit'
			}
		]
	});
}
// Menu객체를 생성
var menu = Menu.buildFromTemplate(arrMenu);

module.exports = menu;
```

2. Electron Application의 진입인 index.js 파일을 아래와 같이 수정 및 추가 합니다.

**index.js**
```
const {app, BrowserWindow, Menu} = require('electron'); //Menu 추가
const appMenu = require('./menu.js'); //추가
...

app.on('ready', () =>{
   ...
   Menu.setApplicationMenu(appMenu); //메뉴를 설정
});
```

4. 실행 후 메뉴 생성을 확인 하고 메뉴를 눌러 봅니다.

## remote(Renderer), BrowserWindow(Main), ipcMain(Main), ipcRenderer

### Renderer process에서 새창을 생성하고, Main process와 통신해봅니다.

[remote](https://electron.atom.io/docs/api/remote/) : Renderer Process에서 Main Process의 모듈을 사용이 가능하게 해주는 모듈

[BrowserWindow](https://electron.atom.io/docs/api/browser-window/) : 창을 생성하고 컨트롤하는 모듈

[ipcMain](https://electron.atom.io/docs/api/ipc-main/) : Main Process에서 Renderer Process로 비동기적으로 통신하는 모듈

[ipcRenderer](https://electron.atom.io/docs/api/ipc-renderer/) : Renderer process에서 Main Process로 비동기적으로 통신하는 모듈

1. 아래와 같이 newwin.js 를 작성합니다.

**newwin.js**
```
const {BrowserWindow} = require('electron');
exports.openWindow = (filename) => {
    let win = new BrowserWindow(
        {
            width : 800
            , minWidth:330
            , height :500
            , minHeight: 450
            , icon: __dirname + '/resources/installer/Icon.ico'
            , webPreferences :{ defaultFontSize : 14}
        }
    );
    win.loadURL(`file://${__dirname}/${filename}.html`);
}
```
2. newwin.html파일을 생성합니다.
3. remote.js 파일을 아래와 같이 생성합니다.

**remote.js**
```
const {remote, ipcRenderer} = require('electron');
const newwin = remote.require('./newwin.js');
const newWinButton = document.createElement('button');

//remote, BrowserWindow 예제
newWinButton.textContent = '새창 열기';
newWinButton.addEventListener('click', () => newwin.openWindow('newwin'));
document.body.appendChild(newWinButton);

document.body.appendChild(document.createElement('div')); //줄바꿈

//ipcMain과 ipcRender API예제
const ipcButton = document.createElement('button');
ipcButton.textContent='Main Process에 신호보내어 창 최대화';
ipcButton.addEventListener('click', () => {
    ipcRenderer.send('sendMsg', 'WindowMax' );
});
ipcRenderer.on('mainMsg',(event, args)=>{
    alert(args);
});

document.body.appendChild(ipcButton);

```
4. index.html파일에 remote.js파일을 포함시킵니다.

**index.html**
```
<script src="remote.js"></script>
```

5. index.js 파일에 아래 코드를 추가합니다.
```
ipcMain.on('sendMsg',(event, args) =>{
       //최대인지 확인후 최대화 또는 최대화 취소
       win.isMaximized() ? win.unmaximize() : win.maximize();
       event.sender.send('mainMsg', 'MainProcess에서 신호보냄');
  });
```

6. 실행 후 화면 안의 버튼을 눌러봅니다.


# 유용한 사이트
## Electron Windows Installer 실행 레지스트리 추가 하는 방법
- [http://cionman.tistory.com/13](http://cionman.tistory.com/13)


## Electron에서 자바스크립트 라이브러리 로딩시 유의 점
관련 글 
- [https://blog.outsider.ne.kr/1170](https://blog.outsider.ne.kr/1170)
- [http://stackoverflow.com/questions/32621988/electron-jquery-is-not-defined](http://stackoverflow.com/questions/32621988/electron-jquery-is-not-defined)

## Electron 관련 유용한 링크
- [Electron 공식 홈페이지 : https://electron.atom.io/](https://electron.atom.io/)</br>
- [Awesome Electron : https://github.com/sindresorhus/awesome-electron](https://github.com/sindresorhus/awesome-electron)</br>
- [Electron Doc 한국어 번역 : https://www.gitbook.com/book/imfly/electron-docs-gitbook/details/ko-KR](https://www.gitbook.com/book/imfly/electron-docs-gitbook/details/ko-KR)</br>
- [Electron Korea 페이스북 그룹 : https://www.facebook.com/groups/electronkorea/?fref=ts](https://www.facebook.com/groups/electronkorea/?fref=ts)