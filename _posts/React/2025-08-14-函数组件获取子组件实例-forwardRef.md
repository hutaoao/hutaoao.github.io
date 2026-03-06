---
title: 函数组件获取子组件实例-forwardRef
date: 2025-08-14
description: 函数组件获取子组件实例（forwardRef）
tags: [React]
categories: [React]
---
# 函数组件获取子组件实例（forwardRef）

<font style="color:rgb(51, 51, 51);">基本用法</font>

const Input = forwardRef((props, ref) => {
  const inputRef = useRef(null)
  return (
    <div ref={inputRef}>hello</div>
  )
})

export default function Index() {
  const ipt = useRef(null)
  const handleClick = useCallback(() => ipt.current.focus(), [ ipt ]);
    
  return (
    <div>
      <Input ref={ipt}></Input>
    </div>
  )
}


### useImperativeHandle

<code><font style="color:rgb(51, 51, 51);">useImperativeHandle</font></code><font style="color:rgb(51, 51, 51);"> 和 </font><code><font style="color:rgb(51, 51, 51);">React.forwardRef</font></code><font style="color:rgb(51, 51, 51);"> 必须是配合使用的。</font><code><font style="color:rgb(51, 51, 51);">useImperativeHandle</font></code><font style="color:rgb(51, 51, 51);"> 可以让你在使用 </font><code><font style="color:rgb(51, 51, 51);">ref</font></code><font style="color:rgb(51, 51, 51);"> 时自定义暴露给父组件的实例值</font>

<code><font style="color:rgb(51, 51, 51);">useImperativeHandle(ref, createHandle, [deps])</font></code>

* <font style="color:rgb(51, 51, 51);">ref：定义 current 对象的 ref </font>
* <font style="color:rgb(51, 51, 51);">createHandle：一个函数，返回值是一个对象，即这个 ref 的 current对象 </font>
* <font style="color:rgb(51, 51, 51);">\[deps]：即依赖列表，当监听的依赖发生变化，useImperativeHandle 才会重新将子组件的实例属性输出到父组件\ </font><font style="color:rgb(51, 51, 51);">ref 的 current 属性上，如果为空数组，则不会重新输出。</font>

<font style="color:rgb(51, 51, 51);"></font>

<font style="color:rgb(51, 51, 51);">一个关于 ref 转发的例子</font>

import React, {
  useState,
  useRef,
  useImperativeHandle,
  useCallback
} from 'react';
import ReactDOM from 'react-dom';

const FancyInput = React.forwardRef((props, ref) => {
  const [ fresh, setFresh ] = useState(false)
  const attRef = useRef(0);
  useImperativeHandle(ref, () => ({
    attRef,
    fresh
  }), [ fresh ]);

  const handleClick = useCallback(() => {
    attRef.current++;
  }, []);

  return (
    <div>
      {attRef.current}
      <button onClick={handleClick}>Fancy</button>
      <button onClick={() => setFresh(!fresh)}>刷新</button>
    </div>
  )
});

const App = props => {
  const fancyInputRef = useRef();

  return (
    <div>
      <FancyInput ref={fancyInputRef} />
      <button
        onClick={() => console.log(fancyInputRef.current)}
      >父组件访问子组件的实例属性</button>
    </div>
  )
}

ReactDOM.render(<App />, root);


ts版示例

/**
 * @component  通用筛选组件
 * @date  2022.9.6
 */

import React, { useState, forwardRef, useImperativeHandle } from 'react'
import { SearchColumnsType } from '@/types/common'
import { DownOutlined, UpOutlined } from '@ant-design/icons'
import { Button, Col, Form, Input, Row, Select, Cascader } from 'antd'

interface IProps {
  columns: SearchColumnsType[]
  onSearch: (e: any) => void
  collapseNumber?: number
}

const AdvancedSearchForm = forwardRef<any, IProps>((props, ref) => {
  const [expand, setExpand] = useState(false)
  const [form] = Form.useForm()
  const {
    columns,
    onSearch,
    collapseNumber = 7
  } = props

  useImperativeHandle(ref, () => ({
    resetOtherFields
  }))

  // 重置一组字段到 initialValues
  const resetOtherFields = (fields: any[]) => {
    form.resetFields(fields)
  }

  const renderItem = (i: SearchColumnsType): any => {
    switch (i.type) {
      // 输入框
      case 'input':
        return <Input placeholder={i.placeholder}/>
      // 选择器
      case 'select':
        return <Select
          options={i.options}
          fieldNames={i.fieldNames}
          placeholder={i.placeholder}
          onChange={i.onChange}
        />
      // 级联选择
      case 'cascader':
        return <Cascader options={i.options} placeholder={i.placeholder}/>
      default:
    }
  }

  const getFields = (): React.ReactElement => {
    const children: any = []
    const columnsLen = columns.length
    const count = columnsLen > collapseNumber ? (expand ? columnsLen : collapseNumber) : columnsLen

    columns.filter((c: any, cIndex: number) => cIndex < count).forEach((i: any) => {
      children.push(
        <Col span={6} key={i.key}>
          <Form.Item
            name={i.key}
            label={i.label}
          >
            {renderItem(i)}
          </Form.Item>
        </Col>
      )
    })
    return children
  }

  const onFinish = (values: any) => {
    onSearch(values)
  }

  return (
    <Form
      form={form}
      name="advanced_search"
      className="ant-advanced-search-form"
      onFinish={onFinish}
    >
      <Row gutter={24}>
        {getFields()}
{% raw %}
        <Col span={6} style={{ marginBottom: 24 }}>
{% endraw %}
          <Button type="primary" htmlType="submit">查询</Button>
          <Button
{% raw %}
            style={{ margin: '0 8px' }}
{% endraw %}
            onClick={() => {
              form.resetFields()
            }}
          >
            重置
          </Button>
          {
            columns.length >= collapseNumber && <a
{% raw %}
              style={{ fontSize: 12 }}
{% endraw %}
              onClick={() => {
                setExpand(!expand)
              }}
            >
              {expand ? <UpOutlined/> : <DownOutlined/>} Collapse
            </a>
          }
        </Col>
      </Row>
      <Row>
      </Row>
    </Form>
  )
})

// displayName 属性用于为 React devtools 扩展的组件提供描述性名称
AdvancedSearchForm.displayName = 'AdvancedSearchForm'
export default AdvancedSearchForm




> 更新: 2022-10-31 16:22:30  
> 原文: <https://www.yuque.com/hutaoao/blog/irbipe>