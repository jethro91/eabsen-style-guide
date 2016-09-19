# Eabsen React/JSX Style Guide

*Style Guide React(termasuk React-Native) dan Redux*
berdasarkan  [AirBnb Style Guide ](https://github.com/airbnb/javascript/blob/master/react/README.md).

Modifikasi Berdaskan prinsip development perusahaan kami.

## Daftar Isi

  1. [Basic Rules](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Naming](#naming)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Refs](#refs)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [Ordering](#ordering)
  1. [`isMounted`](#ismounted)

## Basic Rules

  - Hanya mengandung 1 Komponen Per file.
    - Tetapi bisa lebih dari 1 komponen per file jike memenuhi kondisi [Stateless, or Pure, Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions). eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - Selalu menggunakan JSX syntax.
  - Jangan gunakan `React.createElement` kecuali kamu menggunakannya untuk inisialisasi app atau menggunakan library yang memerlukan mixins.

## Class vs `React.createClass` vs stateless

  - Jika kamu memiliki internal state dan/atau refs, lebih baik gunakan `class extends React.Component` over `React.createClass` kecuali anda terpaksa menggunakan mixins. eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

    ```jsx
    // bad
    const Listing = React.createClass({
      // ...
      render() {
        return (
          <div>{this.state.hello}</div>
        );
      }
    });

    // good
    class Listing extends React.Component {
      // ...
      render() {
        return (
          <div>{this.state.hello}</div>
        );
      }
      }
    }
    ```

    Untuk Konsistensi dan kecepatan development, Selalu menggunakan class daripada komponen fungsi (walaupun komponen tersebut tidak memiki state atau refs):

    ```jsx
    // bad (relying on function name inference is discouraged)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // bad (tidak konsisten untuk kecepatan development)
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }

    // good
    class Listing extends React.Component {
      render() {
        return (
          <div>{this.props.hello}</div>
          );
      }
    }
    ```

## Naming

  - **Ekstensi**: Gunakan `.js` Untuk setiap React components (mengikuti settingan default react-native).
  - **Nama File**: Gunakan PascalCase untuk penamaan file React components. E.g., `ReservationCard.js`.
  - **Nama Referensi**: Gunakan PascalCase untuk React components and camelCase untuk instances-nya(pengisian dalam variabel). eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // bad
    import reservationCard from './ReservationCard';

    // good
    import ReservationCard from './ReservationCard';

    // bad
    const ReservationItem = <ReservationCard />;

    // good
    const reservationItem = <ReservationCard />;
    ```

  - **Nama Komponen**: Gunakan nama file sebagai nama komponen. cth, `ReservationCard.js` harus memiliki referensi dari `ReservationCard`. Kecuali untuk komponen root dalam sebua folder, gunakan `index.js` nama folder menggunakan nama komponen:

    ```jsx
    // bad
    import Footer from './Footer/Footer';

    // bad
    import Footer from './Footer/index';

    // good
    import Footer from './Footer';
    ```

## Declaration

  - Do not use `displayName` for naming components. Instead, name the component by reference.

    ```jsx
    // bad
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // good
    export default class ReservationCard extends React.Component {
    }
    ```

## Alignment

  - Mengikuti alignment styles for JSX syntax, dan auto-indent atom editor. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
      />

    // jika props muat dalam satu baris, biarkan dalam satu baris

    // good
    <Foo bar="bar" />

    // children di-indent seperti ini
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
      >
      <Quux />
    </Foo>
    ```

## Quotes

  - Selalu gunakan (`"`) dalam JSX attributes, tetapi ('') untuk JS lain. eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

  > Why? JSX attributes [can't contain escaped quotes](http://eslint.org/docs/rules/jsx-quotes), so double quotes make contractions like `"don't"` easier to type.
  > Regular HTML attributes also typically use double quotes instead of single, so JSX attributes mirror this convention.

    ```jsx
    // bad
    <Foo bar='bar' />

    // good
    <Foo bar="bar" />

    // bad
    <Foo style={{ left: "20px" }} />

    // good
    <Foo style={{ left: '20px' }} />
    ```

## Spacing

  - Selalu gunakan spasi satu kali untuk self-closing tag.

    ```jsx
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

## Props

  - Selalu gunakan camelCase untuk nama prop.

    ```jsx
    // bad
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // good
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - Selalu tampilkan value boolean walau bernilai `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```jsx
    // good
    <Foo
      hidden
    />

    // bad
    <Foo
      hidden={true}
    />
    ```


  - [Khusua React] Selalu gunakan `alt` prop untuk tag `<img>`. Jika gambar presentational, `alt` daban berisi string kosong atau the `<img>` harus  memiliki `role="presentation"`. eslint: [`jsx-a11y/img-has-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-has-alt.md)

    ```jsx
    // bad
    <img src="hello.jpg" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />

    // good
    <img src="hello.jpg" alt="" />

    // good
    <img src="hello.jpg" role="presentation" />
    ```

  - Jangan Menggunakan value "image", "photo", atau "picture" dalam `<img>` `alt` props. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

  > Why? Screenreaders already announce `img` elements as images, so there is no need to include this information in the alt text.

    ```jsx
    // bad
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - Selalu gunakan valid dan non-abstract [ARIA roles](https://www.w3.org/TR/wai-aria/roles#role_definitions). eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```jsx
    // bad - not an ARIA role
    <div role="datepicker" />

    // bad - abstract ARIA role
    <div role="range" />

    // good
    <div role="button" />
    ```

  - [Khusua React] Jangan menggunakan `accessKey` pada elements. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

  > Why? Inconsistencies between keyboard shortcuts and keyboard commands used by people using screenreaders and keyboards complicate accessibility.

  ```jsx
  // bad
  <div accessKey="h" />

  // good
  <div />
  ```

  - Selalu hindari menggunakan index dalam array map sebagai `key` prop, lebih baik menggunakan ID unik. ([why?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

  ```jsx
  // bad
  {todos.map((item, idx) =>
    <Todo
      {...item}
      key={idx}
    />
  )}

  // good
  {todos.map(item, idx) => (
    <Todo
      {...item}
      key={item.id}
    />
  ))}
  ```

## Refs

  - Selalu gunakan ref callbacks. eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

    ```jsx
    // bad
    <Foo
      ref="myRef"
    />

    // good
    <Foo
      ref={ref => { this.myRef = ref; }}
    />
    ```

## Parentheses

  - Selalu tutupi JSX tags dalam tanda kurung walaupun hanya satu baris. eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)

    ```jsx
    // bad
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // good, when single line
    render() {
      const body = <div>hello</div>;
      return (<MyComponent>{body}</MyComponent>);
    }
    ```

## Tags

  - Selalu self-close tags Ketika tidak memiliki props.children dalam komponen tersebut. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```jsx
    // contoh komponen
    class Foo extends React.Component {
      // ...
      render() {
        return (
          <div className={this.props.className}>
          </div>
        );
      }
    }

    ```jsx
    // bad
    <Foo className="stuff"></Foo>

    // good
    <Foo className="stuff" />
    ```

  - jika komponen anda memiliki prop lebih dari satu bari, tutup tagnya di baris baru. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
      />
    ```

## Methods

  - Bind event handlers setiap fungsi. eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

  > Why? A bind call in the render path creates a brand new function on every single render.

    ```jsx
    // bad
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // good
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

  - Jangan gunakan _ orefix pada setiap fungsi internal komponen React.

    ```jsx
    // bad
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // good
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

  - Pastikan pada fungsi `render` selalu memiliki nilai return . eslint: [`require-render-return`](https://github.com/yannickcr/eslint-plugin-react/pull/502)

    ```jsx
    // bad
    render() {
      (<div />);
    }

    // good
    render() {
      return (<div />);
    }
    ```

## Ordering

  - Urutan dalam `class extends React.Component`:

  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers atau eventHandlers* seperti `onClickSubmit()` atau `onChangeDescription()`
  1. *getter methods untuk `render`* seperti `getSelectReason()` atau `getFooterContent()`
  1. *Optional render methods* seperti `renderNavigation()` atau `renderProfilePicture()`
  1. `render`

  - Cara mendefinisi `propTypes`, `defaultProps`, `contextTypes`, dll...

    ```jsx
    import React, { PropTypes } from 'react';

    class Link extends React.Component {

      constructor(props) {
        super(props);
        this.state = {};
        this.upperCaseText = this.upperCaseText.bind(this);
      }
      upperCaseText(text){
        return text.toUpperCase();
      }
      render() {
        return <a href={this.props.url} data-id={this.props.id}>{upperCaseText(this.props.text)}</a>
      }
    }

    // Posisi deklarasi setelah deklarasi class  
    // untuk mempermudah pengecheckan type jika menggunakan 'connect'-nya Redux

    Link.propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };
    Link.defaultProps = {
      text: 'Hello World',
    };

    //Import dan 'connect' Redux disini

    export default Link;
    ```

  - Urutan untuk `React.createClass`: eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *Optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

## `isMounted`

  - Jangan gunakan `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > Why? [`isMounted` is an anti-pattern][anti-pattern], is not available when using ES6 classes, and is on its way to being officially deprecated.

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html


**[⬆ back to top](#table-of-contents)**
