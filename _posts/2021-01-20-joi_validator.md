---
layout: post
title: "Joi Validator"
categories: nodeJS
author: bn-tw2020
---
* content
{:toc}

## Intro

* 프로젝트를 진행하면서 유효성 검사를 위해 라이브러리를 찾다가 `express-validator` 보다 `joi`가 많이 쓰인다고 발견했다.

* 그래서 joi를 정리하는 글입니다.

![](https://user-images.githubusercontent.com/66770613/147402439-10d53242-494f-41f5-ac02-3af422405225.png)





---

## Validation

* 어떤 데이터의 값이 유효한지, 타당한지 확인하는 것이다.

```
이메일 주소 양식은 test@example.com인데, 회원가입 할 때
이메일 양식이 일치하지 않으면 유효하지 않은 이메일이므로 회원 가입을 막을 수 있음
```

* `이메일 양식이 일치하지 않는다` 라는 것은 UX측면(사용자 경험)에서 사용자에게 편의를 주기 위함이다.
* 사용자가 잘못입력하여 오타가 발생 했으니 다시 한 번 확인을 하라는 것을 의미합니다.
* UI단에서 유효성 검사를 하는 것은 보안적인 측면에서 아무 효과가 없음.

---

### 보안적인 측면에서의 유효성 검사

올바르지 않은 데이터가 서버로 전송되거나, 데이터베이스에 저장되지 않도록 하는 것

---

### joi

[예제 코드 1]
```javascript
app.post("/api/students", (req, res) => {
  let { name } = req.body;

  let student = {
    id: students.length + 1,
    name: name,
  };
  students.push(student);
  res.send(students);
});
```
[문제점]
```
name이라는 변수에 대해서 유효성 검사가 없으므로 어떠한 데이터가 들어올 수가 있다.
```
  
  
[예제 코드 2]
```javascript
app.post("/api/students", (req, res) => {
  let { name } = req.body;
  
  if (!name) {
    res.status(400).send("Name is required...");
    return;
  }
  if (name.length <= 3) {
    res.status(400).send("name must be more than 3 char long");
    return;
  }
  
  let student = {
    id: students.length + 1,
    name: name,
  };
  students.push(student);
  res.send(students);
});
```
[문제점]
```
현재는 name 하나라서 괜찮다고 생각할 수 있지만,
많은 양의 데이터를 받아야 할 경우 체크 해줘야 할 경우가 많아서 코드량이 많아질 수 있다.
```
  
  
[예제 코드 3]
```javascript
const schema = Joi.object({
    name: Joi.string().min(3).max(15).required(),
});

app.post("/api/students", (req, res) => {
  let { name } = req.body;
  
  let result = schema.validate(req.body);
  if (result.error) {
    res.status(400).send(result.error.details[0].message);
    return;
  }
  
  let student = {
    id: students.length + 1,
    name: name,
  };
  students.push(student);
  res.send(students);
});
```

* [예제 코드 3]을 보면 schema 설정을 잘 해준다면 여러 api에 대해서 유효성 검사가 편리할 것 같다.
* joi 라이브러리르 설치하기 위해서는 - `npm install @hapi/joi`
