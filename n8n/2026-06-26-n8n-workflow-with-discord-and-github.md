### Context
n8n을 사용해 첫 번째 워크플로우를 자동화했습니다. 디스코드에서 특정 메시지를 수신하면 트리거가 발생하여 에이전트가 실행됩니다. 이 에이전트는 TIL 형식으로 변환한 후, 생성된 코드를 GitHub에 푸시합니다. 푸시가 완료되면 README.md 파일에 해당 인덱스를 추가하고, 마지막으로 완료 메시지를 발신합니다.

### Core
```javascript
const { defineTrigger } = require('n8n-workflow');

// 디스코드에서 메시지 수신 트리거 설정
defineTrigger({
    node: 'Discord Trigger',
    // 선택할 채널 ID
    channelId: 'YOUR_CHANNEL_ID',
    // 수신 메시지 처리
    onMessage: async (message) => {
        // 메시지를 TIL 포맷으로 변환
        let tilContent = transformMessageToTIL(message);
        // GitHub에 푸시
        await githubPush(tilContent);
        // README.md에 인덱스 추가
        await updateReadmeIndex();
        // 완료 메시지 발신
        sendCompletionMessage();
    }
});

async function githubPush(content) {
    // GitHub에 푸시 로직
}

function transformMessageToTIL(message) {
    // TIL 형식 변환 로직
}

async function updateReadmeIndex() {
    // README.md 업데이트 로직
}

function sendCompletionMessage() {
    // 완료 메시지 발신 로직
}
```

### Insight
n8n을 처음 사용해봤지만, 기본적인 두 가지 개념만 알고 있으면 어렵지 않습니다. 노드의 입력(input)과 출력(output)을 이해하면 쉽게 워크플로우를 만들 수 있습니다. 이 어려운 작업을 알려준 후츠릿에게 감사를 표합니다.