---
title: 자바스크립트로 구현하는 LRU 캐시 교체 알고리즘
category: Data Structure
tags:
  - Data Structure
  - Cache Replacement Algorithm
  - Least Recently Used Cache
  - LRU Cache
last_modified_at: '2025-01-03T09:53:22.791Z'
date: '2025-01-03T09:53:22.791Z'
---

# 자바스크립트로 구현하는 LRU 캐시 교체 알고리즘

### 캐시 교체 알고리즘(Cache Replacement Algorithm)

**캐시**는 향후의 동일한 데이터에 대한 요청을 빠르게 처리할 수 있도록 데이터를 임시로 저장하는 하드웨어 또는 소프트웨어 컴포넌트를 의미한다.  

이때 저장되는 데이터를 **캐시 데이터**라고 하며, 이 데이터는 빠르게 접근할 수 있도록 실제 데이터가 영구적으로 저장된 저장소와는 별도의 공간인 **캐시 메모리**에 임시로 저장된다. 

대부분의 캐시 메모리는 유한한 자원으로, 효율적인 관리를 위해서는 제한된 공간 내에서 가장 자주 사용되는 데이터를 유지하고, 덜 사용되는 데이터는 교체하여 성능을 최적화하는 작업을 필요로 하는데, 이를 위해 다양한 **캐시 교체 알고리즘**이 활용된다.

#### 대표적인 캐시 교체 알고리즘 

- FIFO(First In, First Out)
- LRU(Least Recently Used)
- LFU(Least Frequently Used)
- MFU(Most Frequently used)
- NUR(Not Used Recently)
- Random

## LRU Cache

LRU(Least Recently Used) 캐시는 가장 최근에 액세스한 데이터가 다시 액세스될 가능성이 높다고 가정 하에 작동한다. 캐시 공간이 가득 찼을 경우, **가장 오랫동안 사용되지 않은 데이터를 제거하여 새로운 데이터를 저장할 공간을 확보(swap out)한다.**

LRU는 대부분의 일반적인 사용 패턴에서 잘 동작하며, 특히 동일한 데이터를 반복적으로 요청하는 패턴에서 효과적이다. 

### LRU Cache의 구현 
 - **인스턴스 속성**
     - `LRUCache.prototype.constructor`

 - **인스턴스 메서드**
     - `LRUCache.prototype.get()`  
        주어진 키에 해당하는 값을 반환하거나 값이 없다면 `undefined`을 반환합니다.
         
     - `LRUCache.prototype.put()`  
        LRUCache객체에서 전달된 키의 값을 설정합니다. Map객체를 반환합니다.

#### Map을 사용한 구현

- JavaScript(ES6) & Prototype

    ```js
    /**
     * @param {number} capacity
     */
    const LRUCache = function(capacity) {
        this.capacity = capacity;
        this.cache = new Map(); 
    };

    /** 
     * @param {number} key
     * @return {number | undefined}
     */
    LRUCache.prototype.get = function(key) {
        if(this.cache.has(key)){
            const value = this.cache.get(key);   
                
            this.cache.delete(key);    
            this.cache.set(key, value); 
                
            return value;
        }
            
        return;
    };

    /** 
     * @param {number} key 
     * @param {number} value
     * @return {void}
     */
    LRUCache.prototype.put = function(key, value) {
        if(this.cache.has(key)){
            this.cache.delete(key);     
        } 
            
        if(this.capacity){
            if(this.cache.size >= this.capacity){
                const oldest = this.cache.keys().next().value; 
                this.cache.delete(oldest);
            }
            
            this.cache.set(key, value); 
        }
    };
    ```

- Typescript & Class

    ```ts
    export interface ILRUCache<TKey, TValue> {
        get(key: TKey): TValue | undefined;
        put(key: TKey, value: TValue): void;
    }

    class LRUCache<TKey, TValue> implements ILRUCache<TKey, TValue> {
        private cache: Map<TKey, TValue>;
        private capacity: number;
        constructor(capacity: number){
            this.capacity = capacity;
            this.cache = new Map(); 
        }
        
        get(key: TKey): TValue | undefined {
            if(this.cache.has(key)){
            const value = this.cache.get(key);
            this.cache.delete(key);    
            this.cache.set(key, value!); 
            
            return value;   
            }
            return;
        }
        
        put(key: TKey, value: TValue){
            if(this.cache.has(key)){
                this.cache.delete(key);     
            } 
            
            if(this.capacity > 0){
                if(this.cache.size >= this.capacity){
                    const oldest = this.cache.keys().next().value; 
                    oldest && this.cache.delete(oldest);
                }
                
                this.cache.set(key, value); 
            }
        }
    }
    ```
