# useForm register 자식 컴포너트에 전달하기

## 1. 개요

`useForm` 모듈/라이브러리의 `register`는 `input` 태크에 사용되는 것으로
해당 `input`이 무엇을 의미하는지 나타낸다. 간단한 사용법은 아래와 같다.

```jsx
return (
  <form>
    <input {...register("name")} />
  </form>
);
```

이런 `register`를 타입스크립트를 사용하여 자식 컴포넌트로 보내는 방법에 대해 정리한다.

---

## 2. 타입스크립트를 사용하지 않고 보내기

```jsx
// 부모 컴포넌트
const Parents = () => {
  const { register } = useFrom();
  return (
    <form>
      <Children register={register} />
    </form>
  );
};

// 자식 컴포넌트
const Children = ({ register }) => {
  return <input {...register("name")} />;
};
```

타입스크립트를 사용하지 않는다면 간단하게 `register`를 그대로 자식 컴포넌트로
보내고 자식컴포넌트에서는 `register`를 받은 후 그대로 사용하면 된다.

---

## 3. 타입스크립트를 사용하여 보내기

```tsx
// 부모 컴포넌트
const Parents = () => {
  const { register } = useFrom();
  return (
    <form>
      <Children register={register("name")} />
    </form>
  );
};

// 자식 컴포넌트
interface IProps {
  register: UseFormRegisterReturn;
}

const Children = ({ register }: IProps) => {
  return <input {...register} />;
};
```

타입스크립트를 사용한다면 위와 같은 방법으로 자식 컴포넌트로 전달해야 한다.

- 부모 컴포넌트에서 `register`를 보낼 때 첫번째 매개변수를 지정해줘야 한다.
- 자식 컴포넌트에서 `register`를 받을 땐 `UseFormRegisterReturn`이라는
  타입을 정해줘야 한다.
- 자식 컴포넌트에서 `register`를 사용할 땐 구조분해 할당으로 받아 객체로 바꾸어
  사용해야 한다.

---

📅 2022-10-03
