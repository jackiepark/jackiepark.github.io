---
layout: post
title: how to test react components
---

## React Component Test 의 이점

> test 를 잘 하기 위해서는 component depth 를 깊게 만들 수가 없습니다.
depth 가 깊어지면 테스트 코드를 작성하기가 힘들기 때문입니다. 이에 Test Code 를 함께 작성하면 자연스럽게 좋은 구조를 유지할 수 있습니다. depth 가 얕아지면 불필요한 mock 이 사라집니다. mock 은 고통의 시작입니다.
> Component 를 테스트한다고 적었지만 실제로는 Component 의 로직을 테스트하는데 집중합니다. 이는 완벽하게 오류를 잡아낸다기 보다 정말로 하지 말아야할 실수들을 잡아내는데 목적이 있습니다. (List 의 Item 갯수가 0일때 Empty Component 를 그린다등이 이런예에 들어갑니다)

## Test 도구

> 유닛 테스트란 모름지기 수행속도가 빨라야 의미가 있다고 생각합니다. 이전에도 안드로이드 테스트를 위해 로보레트릭을 설정하였으나 인내심의 한계를 느끼게 만드는 수행속도 때문에 포기한 경험이 있습니다. 때문에 Jest나 Karma 를 배제하고 mocha 로만 작성하기를 권합니다. 
> mocha 는 대표적인 javascript test runner 이면서 framework 입니다. 원하는 assertion 라이브러리를 선택해서 쓸 수 있고 동기, 비동기 테스를 모두 작성할 수 있습니다.
> 물론 CI 용으로 karma 나 다른 도구를 사용합니다.

## Test with CSS

> 실제 작성되는 코드는 component 별로 css 를 import 하게 되는데 css 를 import 하게 되면 웹상에 sample code 들은 동작하지 않게됩니다. 여기서 맨붕을 겪으신 분이 많이 있으실 것 같습니다. 다양한 해결방법이 있지만 제가 추천하는 방법은 css 를 ignore 하는 방법입니다. 특정 div 의 배경색이 빨간색인지를 체크하는 것은 의미가 없다고 생각합니다. 전체적인 동작과 화면구성은 기획자가 확인할 수 있는 example 코드를 따로 작성하시길 추천합니다.
> 중복코드 발생을 최소화하기 위해 mocha.opts 와 helper 코드를 아래처럼 작성합니다.

```js
// mocha.opts
--compilers js:babel-register
--recursive
--timeout 10000
--require ./test/test-helper.js
--require ignore-styles
--full-trace
```

```js
// helper.js
import chai from 'chai';
import sinon from 'sinon';
import sinonChai from 'sinon-chai';
import jsdom from 'mocha-jsdom';
import chaiEnzyme from 'chai-enzyme';

chai.use(sinonChai);
chai.use(chaiEnzyme());

global.chai = chai;
global.expect = chai.expect;
global.sinon = sinon;
global.jsdom = jsdom;
global.should = chai.should();

// !!! if you don't want to global, export { chai } and import { chai } from 'test-helper'
```

## Test 순서

> Test 는 Component 작성 시와 반대로 leaf code 부터 작성하는게 더 쉽습니다.

## Enzyme

> React Component Test Utilities 로 ReactTestUtils 대신 Enzyme 를 사용합니다. ReactTestUtils 은 사용하기 너무 괴롭습니다.

```js
// from
const component = TestUtils.renderIntoDocument(<Counter counter={1} {...actions} />)
// to
const wrapper = mount(<Counter counter={1} {...actions} />)

// from
const buttons = TestUtils.scryRenderedDOMComponentsWithTag(component, 'button')
// to
const buttons = wrapper.find('button')


// from
p = TestUtils.findRenderedDOMComponentWithTag(component, 'p')
// to
const p = wrapper.find('p')
expect(p).to.have.length(1)

// from
TestUtils.Simulate.click(button)
// to
button.simulate('click')
```

## Shallow

> Enzyme 의 mount 보다 shallow 를 사용하길 권장합니다. 이유는 계속 동일합니다. shallow 은 one level 렌더링을 합니다. dom 을 이용하지 않기 때문에 속도도 빠릅니다. 하위 컴포넌트는 컴포넌트 이름 그대로 find 할 수 있어 좋습니다.

```js
//Items.js
describe('render by shallow', () => {
    let props = {};
    it('should return none', () => {
      props.items = [];
      const wrapper = shallow(<Items />);
      expect(wrapper.find(Item)).to.not.exist;
      expect(wrapper).to.contain(<div></div>);
    });

    it('should return Item', () => {
      props.items = mockItems();
      const wrapper = shallow(<Items {...props} />);
      expect(wrapper.find(Item)).to.exist;
      expect(wrapper.find(Item)).to.have.length(props.items.length);
    });
  });
```

## Mount

> Enzyme 가 version 2.0.0 이 되면서 jsdom helper method 가 없어졌습니다. 직접 구성해서 사용합니다.