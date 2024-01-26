---
layout: post
title: "React - puppeteer를 활용한 PDF 다운로드 기능 개발"
date: 2021-04-28 11:20:00 +0100
categories: ["React.js"]
---

리액트로 M심의봇 프로젝트를 이관하던 중, 기존 서비스에서 지원하던 기능인 '화면 PDF로 다운받기' 기능을 구현하기 위해 다양한 react 패키지를 활용해
시도해 보았지만, 결코 쉬운 일이 아니였다.
보통 html2canvas와 jsPDF 패키지를 활용한 방법을 사용하는 것 같은데, 내 경우 외부 서버에서 링크되어 있는 이미지들이 다수 포함되어 있는 화면을
캡쳐하여 PDF로 내보내주어야 하기 때문에, 다른 방법을 사용하여야 한다고 판단했다.
&nbsp;
&nbsp;
이 문제를 해결하기 위해 나는 puppeteer를 활용하기로 하였다. 우선, node.js로 구현되어 있는 백엔드에 puppeteer를 통한 화면의 내용을 buffer 형태로
만들어 frontend로 반환하는 api를 구현하였다.
&nbsp;
&nbsp;

```
// backend -> utilTestController.js
module.exports.printPDF = async (req, res) => {
  const params = req.body;
  try {
    tmpInnerHtml = params.div;
    const browser = await puppeteer.launch({ headless: true });
    const page = await browser.newPage();
    await page.goto('http://localhost:3000/pdf', { waitUntil: 'networkidle0' });
    const pdf = await page.pdf({ format: 'A4' });
    await browser.close();
    res.set({ 'Content-Type': 'application/pdf', 'Content-Length': pdf.length });
    res.status(200).send(pdf);
  } catch (err) {
    logger.error(err);
    res.status(500).send(err.message);
  }
};
```

&nbsp;
&nbsp;
그 다음, 반환받은 buffer를 파일 형식으로 떨어트리도록 frontend에 함수를 구현하였다.
&nbsp;
&nbsp;

```
// frontend -> ViewSpecificModal.js
const getPDF = () => {
  const config = {
    method: 'post',
    url: 'http://10.53.39.106:4000/api/printPDF',
    headers: { 'Accept': 'application/pdf' },
    responseType: 'arraybuffer',
    data : {div : document.querySelector('#specificView').innerHTML}
  };

  return axios(config);
}

const downloadPdf = () => {
  return getPDF() // API call
  .then((response) => {
    const blob = new Blob([response.data], {type: 'application/pdf'});
    const link = document.createElement('a');
    link.href = window.URL.createObjectURL(blob);
    link.download = `your-file-name.pdf`;
    link.click();
  })
  .catch(err => console.log('123'))
}
```

&nbsp;
&nbsp;
이렇게 프론트엔드, 백엔드에 구현하게 되면, 백엔드 API의
await page.goto('https://google.com', { waitUntil: 'networkidle0' });
에서 볼 수 있듯이 goto함수 내부에 파라미터로 주어진 url을 헤드리스로 열고, 그 내용을 pdf buffer 형태로 만들게 된다.
이 때, 나는 한 가지 작업을 더 해주어야 한다는 것을 알게 되었다. 나는 화면의 일부 내용만을 pdf로 다운받고자 하기 때문에,
일부 내용을 떼어 별도 화면을 만들어주어야 한다는 것이다. 따라서, 나는 React의 react-router-dom 패키지를 활용하여
메인 경로가 아닌 /pdf 경로를 별도로 만들어주고, 그 화면에 dom을 전달하여 화면을 구성하도록 하였다.
&nbsp;
&nbsp;

```
// frontend -> App.js
import PageTop from './components/PageTop';
import Pdf from './components/Pdf';
import { Route, Switch, BrowserRouter as Router } from "react-router-dom";
import { Grid } from '@material-ui/core';
import './style/PageTop.css'

function App() {
  return (
    <Router>
      <Switch>
        <Route exact path="/">
            <Grid container spacing={1}>
              <PageTop defaultPageName={'검출 실행'} defaultPageNum={0} />
            </Grid>
        </Route>
        <Route path="/pdf" component={Pdf}/>
      </Switch>
    </Router>
  );
}

export default App;
```

&nbsp;
&nbsp;
이후, props 형태의 데이터 전달을 시도해 보았지만 headless chrome이 화면을 열 때는 해당 화면이 데이터를 가지고 있지 않는 문제가 발생했다.
따라서, pdf 다운로드 버튼을 누르는 시점에 dom의 innerHTML string을 백엔드에 전달하고, 화면이 열릴 때 그 데이터를 불러와 화면을 그리도록 하였다.
백엔드의 printPDF API가 구현되어 있는 파일에 tmpInnerHtml라는 전역변수를 설정하고 tmpInnerHtml = params.div; 의 형태로
파라미터를 저장하였다가, 화면이 열릴 때 다음과 같은 형태로 해당 내용을 불러와 화면을 그린다.
&nbsp;
&nbsp;

```
// frontend -> Pdf.js
const Pdf = () => {
  const [tmpInnerHtml, setTmpInnerHtml] = useState('');

  useEffect(()=> {
    const config = {
      method: 'post',
      url: 'http://10.53.39.106:4000/api/getInnerHtml',
      headers: {},
      data : {div : '123'}
    };

    axios(config)
      .then((response) => {
        setTmpInnerHtml(response.data.tmpInnerHtml);
        document.querySelector('#mainScreen').childNodes[5].childNodes[0].style.height = '100%';
        document.querySelector('#mainScreen').childNodes[5].childNodes[0].style.overflow = 'visible';
      })
      .catch((error) => {
        console.log(error);
      });
  }, [])

  return (
    <div>
      <div id='mainScreen' dangerouslySetInnerHTML={{__html : tmpInnerHtml}} />
    </div>
  );
};
```

&nbsp;
&nbsp;
아직 정확히 파악하지 못한 문제로 인해 다운받은 PDF의 화면 구성이 다소 엉망이긴 하지만, 이런 과정을 통해 원하는 내용을 PDF로 다운로드 받는 데 성공하였다.
이제 화면 구성 정리를 마치면 원하는 기능 구현은 마무리될 것 같다.
