# @PrimaryKeyJoinColumn์ด๋?

### ๐ ์ ์

JPA 1.0์์๋ **OneToOne๊ด๊ณ ๋๋ ManyToOne๊ด๊ณ**์์ Id๋ฅผ ์ฌ์ฉํ  ์ ์์์ผ๋ฏ๋ก ์ฌ์ฉํ๋ ๊ฒ์ด `@PrimaryKeyJoinColumn`์ด๋ค.

`@Id` + `@JoinColumn`๊ณผ `@PrimaryKeyJoinColumn`์ **์ ์ฌ**ํ๋ค. ํ์ง๋ง, ์์์ ์ด ๋ฐฉ์(`Id + JoinColumn`)์ด ๋ ๋ชํํด์ ์ถ์ฒํ๋ค.

ํ์ฌ `@PrimaryKeyJoinColumn`์ **์์๊ด๊ณ ๋งคํ**(JOINED)์์ ์ฌ์ฉ๋๊ณ ์๋ค.