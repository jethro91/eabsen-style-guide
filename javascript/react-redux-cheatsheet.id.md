# Eabsen React/JSX Redux ES6 Cheat Sheet

*CheatSheet unuk React-Redux shema ES6*

## Daftar Isi

  1. [Skeleton](#skeleton)
  1. [Tipe PropTypes](#tipe-proptypes)
  1. [Map Array](#map-array)
  1. [Map Object](#map-object)

## Skeleton

  Skeleton Harus sesuai urutan dibawah ini

  ```jsx
  import React, { PropTypes } from 'react';

  // Import fungsi Helper javascript
  // Import React Component

  // Init constanta


  class Link extends React.Component {

    constructor(props) {
      super(props);
      this.state = {};
      this.toPath = this.toPath.bind(this);
      this.upperCaseText = this.upperCaseText.bind(this);
    }
    componentWillMount() {
      //optional
    }
    componentDidMount() {
      //optional
    }
    componentWillReceiveProps(nextProps) {
      //optional
    }
    shouldComponentUpdate(nextProps, nextState) {
      //optional
      return this.props !== nextProps
    }
    componentWillUpdate(nextProps, nextState) {
      //optional
    }
    componentDidUpdate(prevProps, prevState) {
      //optional
    }
    componentWillUnmount() {
      //optional
    }
    toPath(){
      //Selalu bind di constructor
      //dari redux routerActions
      this.props.pushPath(this.props.url , this.props.currentQuery);
    }
    upperCaseText(text){
      //Selalu bind di constructor
      return text.toUpperCase();
    }
    render() {
      return (
        <a onCLick={this.toPath} href={this.props.url} data-id={this.props.id}>
          {this.upperCaseText(this.props.text)}
        </a>
      );
    }
  }

  Link.propTypes = {
    id: PropTypes.number.isRequired,
    url: PropTypes.string.isRequired,
    text: PropTypes.string,
  };
  Link.defaultProps = {
    text: 'Hello World',
  };

  //Import fungsi redux dan 'connect' Redux disini
  //Optional jika menggunakan Redux
  //tanpa Redux cukup export default Link

  import {actions as routerActions} from 'src/modules/router';

  import {selectors as routerSelectors} from 'src/modules/router';

  import {connect} from 'react-redux';

  Vonst makeMapActionsToProps = Object.assign({}, routerActions);

  const makeMapStateToProps = () => {
    const mapStateToProps = (state, props) => {
      return {
        currentQuery: routerSelectors.getQuery(state, props),
        currentPathname: routerSelectors.getPathname(state, props),
      };
    }
    return mapStateToProps;
  }

  export default connect(makeMapStateToProps, makeMapActionsToProps)(Link)
  ```

## Tipe PropTypes

  - Proptypes Bawaan:

  ```jsx
  Link.propTypes = {
    email:      PropTypes.string,
    seats:      PropTypes.number,
    settings:   PropTypes.object,
    callback:   PropTypes.func,
    isClosed:   PropTypes.bool,
    any:        PropTypes.any,
  };

  ```

  - Wajib Diisi:
  Tambahkan .isRequired.

  ```jsx
  Link.propTypes = {
    email:      PropTypes.string.isRequired,
  };

  ```

  - Jika React Component
  Gunakan .element, .node.

  ```jsx
  Link.propTypes = {
    children:   PropTypes.element, // react element
    node:       PropTypes.node,    // num, string, element
                                   // ...or array of those
  };

  ```

  - Enumerables
  Gunakan .oneOf, .onOfType

  ```jsx
  Link.propTypes = {
    enum:     PropTypes.oneOf(['M','F']),  // enum
    union:    PropTypes.oneOfType( // any
                [        
                  PropTypes.string,
                  PropTypes.number
                ]
              ),
  };

  ```

  - Arrays dan objects
  Gunakan .array[Of], .object[Of], .instanceOf, .shape.

  ```jsx
  Link.propTypes = {
    array:    PropTypes.array,
    arrayOf:  PropTypes.arrayOf(PropTypes.number),
    object:   PropTypes.object,
    objectOf: PropTypes.objectOf(PropTypes.number),

    message:  PropTypes.instanceOf(Message),

    object2:  PropTypes.shape({
        color:  PropTypes.string,
        size:   PropTypes.number
    }),
  };

  ```

## Map Array

```jsx
  render(){
    //contoh data
    let listDataArr = [
      {
        id: '1',
        name: 'Tifa'
      },
      {
        id: '2',
        name: 'Rinoa'
      }
    ];

    return(

      {listDataArr.map((item) => {
        return(
          <div key={item.id}>
            ID : {item.id}
            Value: {item.name}
          </div>
          );
      })}

    );
  }



```

## Map Object

```jsx
  render(){
    //contoh data
    let listDataObj = {
      '1': {
        name: 'Tifa'
      },
      '2': {
        name: 'Rinoa'
      }
    };

    return(

      {Object.keys(listDataObj).map((key) => {
        return (
          <div key={key}>
            ID: {key}
            Value: {listDataObj[key].name}
          </div>
        );
      })}

    );
  }

```

**[⬆ back to top](#daftar-isi)**
