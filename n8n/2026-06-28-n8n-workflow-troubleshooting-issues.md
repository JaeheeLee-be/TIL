### Context
n8n에서 GitHub API를 사용할 때 한글 파일명이 인코딩 문제로 인해 자동화 워크플로우에서 실패하였다. 이러한 문제를 해결하기 위해 파일명을 영문으로 변경하고, 중복 파일 생성 시 SHA 에러를 방지하기 위해 파일명에 HHmm 시간을 추가했다. 

Retry On Fail은 Push TIL 및 AI Agent 노드에 각각 2회 수행하며 3초 간격으로 설정하였다. 에러 발생 시 Discord로 알림을 보내기 위해 On Error를 Continue using error output으로 변경하였다. 그러나 OpenAI 모델에서 JSON 출력이 간헐적으로 깨지는 문제가 발생했다. 이를 해결하기 위해 System Message의 마지막에 "순수 JSON만 출력하라"는 지시를 추가하였다.

### Core
```javascript
// GitHub API로부터 파일명 변경을 위한 기본 설정
let fileName = 'my-new-filename';
let timestamp = new Date().toISOString().slice(0, 16).replace(':', '');
fileName += `_${timestamp}`; // HHmm 추가

// GitHub API 호출 예시
const response = await axios.get(`https://api.github.com/repos/{owner}/{repo}/contents/${fileName}`);
console.log(response.data);
```  

// Discord로 에러 알림 보내기
const webhookUrl = 'https://discord.com/api/webhooks/{webhook.id}/{webhook.token}';
const errorMsg = { content: '워크플로우가 실패했습니다.' };
axios.post(webhookUrl, errorMsg);

// OpenAI 호출 시 JSON 출력에 대한 강화 지시
const openAiResponse = await openai.chat.completions.create({
  messages: [
    {role: 'system', content: '최종 출력은 순수 JSON 형식이어야 합니다.'},
    {role: 'user', content: '질문 내용'}
  ]
});
console.log(openAiResponse);
```

### Insight
이번 워크플로우 트러블슈팅에서는 파일명 문제와 오류 자동 처리 방법을 성공적으로 적용하여 효과적으로 에러를 관리할 수 있었다. 특히 OpenAI의 후처리 지시 추가가 문제 해결에 큰 도움이 되었다. GitHub API를 이용한 자동화는 충분한 요소로 구성된 워크플로우를 통해 안정화되었으며, 앞으로 구체적인 상황에 맞춘 최적화 방안도 마련할 예정이다.

**출처:** [n8n Documentation](https://docs.n8n.io), [GitHub API](https://docs.github.com/en/rest), [OpenAI API](https://platform.openai.com/docs/api-reference/overview)