# Api_Validation

### API 요청시 기본적인 매개변수 검증과 같은 처리를 담당하기 위한 모듈입니다.

```js

const apiValidation = async (url ,
                             httpRequestKind,
                             params,
                             paramsValidationFunction,
) => {
    if (paramsValidationFunction != null) {
        const paramValidationResult = await paramsValidationFunction(params);
        if (!paramValidationResult.isResult)
            return {
                isResult: false,
                data: paramValidationResult.message
            }
    }

    switch (httpRequestKind) {
        case "post": {
            return await Api.post(url , params).then( async result => {
                const resultCheck = await resultValidation(result).catch(reason => {
                    return {
                        isResult: false,
                        data: reason.message
                    }
                });

                if (resultCheck)
                    return {
                        isResult: true,
                        data: result.response
                    }
            })
        }
    }


}

export default apiValidation;
```
